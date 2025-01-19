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
* Within the power shell - once
  * usbipd list 
  * usbipd bind --busid 1-3
* Within the power shell - every time
  * usbipd attach --wsl --busid 1-3

# Connecting the board within Visual Studio Code
After the Pico board has been attached, it can be connected using the Visual Studio Code via the following commands
* WSL: Connect to WSL 
* Open the desired folder within the Ubuntu instance
* MicroPico: connect
