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
