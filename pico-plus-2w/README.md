# Pimoroni Pico Pus 2W

* [Board](https://shop.pimoroni.com/products/pimoroni-pico-plus-2-w?variant=42182811942995)
* [Firmware](https://github.com/pimoroni/pimoroni-pico-rp2350)
* [Micropython Examples](https://github.com/pimoroni/pimoroni-pico-rp2350/tree/main/micropython/examples)

# Attaching the board to WSL2
As I'm using WSL2 (Ubuntu) as my development environment I need to attach the Pico board as USB device (see [connect USB devices](https://learn.microsoft.com/en-us/windows/wsl/connect-usb)).

For short
* Start a WSL2 instance.
* Start power shell in admin mode.
* Connect the Pico board.

Power Shell (once)
``` 
usbipd list 
usbipd bind --busid 1-3
```

Power Shell (every time)
```
usbipd attach --wsl --busid 1-3
```

# Connecting the board within Visual Studio Code
After the Pico board has been attached, it can be connected within Visual Studio Code (see [Developing in WSL](https://code.visualstudio.com/docs/remote/wsl), [Micro Pico extension](https://marketplace.visualstudio.com/items?itemName=paulober.pico-w-go), [Raspberry Pi Pico extension](https://marketplace.visualstudio.com/items?itemName=raspberry-pi.raspberry-pi-pico)).


Connect to the WSL instance
```
WSL: Connect to WSL 
```

Open the projecte folder within the Ubuntu instance

Connect the Pico board
```
 MicroPico: connect
```
