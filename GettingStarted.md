# Introduction #

This is a simple project to connect a PS/2 mouse to an Apple ADB system. Right now it is only developed and tested on an Apple IIgs but it might work with ADB Macs as well.

# Warning #
This code is pre-alpha quality. I threw it together in a couple days and it completely neglects certain aspects of the ADB bus such as collision detection. Lots of debugging bits, used and unused, remain in the code. **USE THIS CODE AT YOUR OWN RISK! ASSUME IT WILL CATCH YOUR COMPUTER ON FIRE!**

# Details #

## Hardware Configuration ##
This project should work on any Arduino with an ATMega8 series chip (ATMega8/ATMega168/ATMega328).

First, hook up the ADB and PS/2 ports to the Arduino as follows:

### PS/2 ###
|Name|PS/2 pin|Arduino pin|AtMEGA pin|
|:---|:-------|:----------|:---------|
|DATA|1 |4 |6 (PD4)|
|GND|3 |GND|8+22|
|VCC|4 |+5V|7+21|
|CLOCK|5 |3 |5 (PD3)|


### ADB ###
|Name|ADB pin|Arduino pin|AtMEGA pin|
|:---|:------|:----------|:---------|
|ADB|1 |8 |14 (PB0)|
|VCC|3 |+5V|7+21|
|GND|4 |GND|8+22|

The system will draw power from the ADB bus and supply it to the Arduino and the PS/2 mouse.

Note that you can substitute other pins for PS/2 clock and data if you like, just change the `#define PS2CLOCK 3` and `#define PS2DATA 4` lines in the code. However, you **MUST NOT** change the ADB pin. It **MUST** be connected to Arduino pin 8. This is because the timer interrupt used only works on pin 8.

If you find that your mouse isn't working, particularly if it is a USB mouse with a PS/2 to USB adapter, add 10k pullup resistors to the clock and data lines.

## Software configuration ##

You will need to download the PS/2 mouse library from https://github.com/kristopher/PS2-Mouse-Arduino and place it in your Arduino/libraries folder. Rename it to something without dashes (like "PS2") or Arduino will complain.

If you are using Arduino 1.0 or above, edit the file PS2Mouse.cpp and change the line `#include "WConstants.h"` to `#include "Arduino.h"` .

After that you should be good to go. Upload the code to your Arduino, plug in the PS/2 mouse and the ADB bus, and enjoy!

## Bugs ##
If the PS/2 mouse is not plugged in when the Arduino starts up, the code will hang. Reset your Arduino or power cycle your computer after plugging the PS/2 mouse and it should start working.

This code assumes your Arduino is running at 16MHz. It will not work at other speeds.