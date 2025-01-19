# Smart Energy Meter
I'm using this project to monitor the energy consumption of my Vaillant heating pump.

## Hardware
* [ORNO OR-WE-517 Energy Meter](https://www.orno.pl/en/energy-meters-with-mid/350-3-phase-multi-tariff-energy-meter-with-rs-485-80a-4-5-modules-din-th-35mm-5902560322422.html)
* [USB To RS485 converter](https://www.waveshare.com/wiki/USB_TO_RS485)
* [Raspberry Pi](https://www.raspberrypi.com/products/raspberry-pi-1-model-b-plus/) as IoT device 
* [Intel NUC 5](https://www.intel.com/content/www/us/en/ark/products/series/129708/intel-nuc-kit-with-5th-generation-intel-core-processors.html) as server for my containerized software.

## Software
* Python with [minimalmodbus](https://github.com/pyhys/minimalmodbus) and [paho-mqtt](https://github.com/eclipse-paho/paho.mqtt.python) packages to read data from the energy meter and to publish this data via MQTT. The Python program is running on my Rasberry Pi.
* [HiveMQ Community](https://github.com/hivemq/hivemq-community-edition) MQTT broker running as container on my NUC.
* [PostgreSQL](https://www.postgresql.org/) database running as container on my NUC.
* [Quarkus](https://quarkus.io/) service consuming (quarkus-messaging-mqtt) and persisting (quarkus-hibernate-orm-panache) the data. This service is also running as container on my NUC.

Further details will follow shortly.


## Reading data from the energy meter
While there are many sources and examples for minimalbodbus and RS485, it was not that easy to find the correct parametersv the for OR-WE-517 energy meter.  Credits to "charakterkopf" for his [Post](https://forum.iobroker.net/topic/30953/abfrage-orno-or-we-516-517-modbus-evtl-script-vorhanden) (German). It is also important to convert the read data bytes (IEEE 754 standard).

I have implemented this functionalty in the following Python scripts.
```python
# converters.py
import struct
import binascii
import serial

def convert_array_to_float(value):  #Array of int ( 4 byte) to float according IEEE 754
    converted=str(hex(value))
    converted=converted.replace('0x', '')
    if converted=='0':
       converted='00000000'
    unpacked=struct.unpack('>f', binascii.unhexlify(converted))[0]
    return (unpacked)
```

Register 0100 Hex (256) holds the total active energy consumed.

```python
# energymeter.py
import minimalmodbus
import serial
from converters import convert_array_to_float

class EnergyMeter:

    def __init__(self, name, port='/dev/ttyUSB0'):
        #adapter for OR-WE-516
        self.name=name
        self.smartmeter = minimalmodbus.Instrument(port, 1,) # port name, slave address (in decimal)
        self.smartmeter.serial.baudrate = 9600 # Baud
        self.smartmeter.serial.bytesize = 8
        self.smartmeter.serial.parity   = serial.PARITY_EVEN # vendor default is EVEN
        self.smartmeter.serial.stopbits = 1
        self.smartmeter.serial.timeout  = 0.6  # seconds
        self.smartmeter.mode = minimalmodbus.MODE_RTU   # rtu or ascii mode
        self.smartmeter.clear_buffers_before_each_transaction = False
        self.smartmeter.debug = False # set to "True" for debug mode


    def read_total_energy(self) :
        value = convert_array_to_float(self.smartmeter.read_long(256, 3, False, 0))
        return (value)
```
