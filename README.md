# Dual USB Controller adapter CPC
This project is a version of the Dual USB Controller adapter project dedicated to the Amstrad CPC.
It is an interface board to connect two USB controllers on the DB9 joystick port of the Amstrad CPC.
Supported USB devices : mouse, keyboard, wired xbox360 and PS3 gamepad ; other HID controllers should also work but button placement is device dependant. Composite devices are not supported.

Working so far :
- two USB ports for two players games
- DB9 output plug to the joystick port of the CPC
- DB9 input to daisy chain an Atari controller
- Need external 5V 0.5A supply, WITH SAME GND AS THE CPC
- USB HID devices like mouse, keyboard or gamepad
- Mouse support with AMX compatibility
- Wired XBOX 360, PS3 and HID gamepad support. 3 buttons (button B is both on B and X, button 3 is on Y).
- Autofire on right trigger
- Auto-Left-Right on left trigger, for "sports" games :P
- platform mode with jump mapped on a button

# Disclaimer
This is a hobbyist project, it comes with no warranty and no support. Also remember that the machines are about 30 years old and may fail because of such hardware expansions.

I publish my work under the CC-BY-NC-SA license. The content of this repository is the result of weeks of work, requiring investment in tools, parts, prototypes, and risky tests on his own equipment. For this reason the author does not want third parties to sell products and keep all the profit, usually without even offering support to their customers. If you see people selling this without the consent of the author, don't buy from them.

If you find it useful and want to reward me : I am always looking for Amiga/Amstrad CPC hardware to repair and hack, please contact me.

# Power, ESD and other Warning
The adapter requires an external 5V supply WITH THE SAME GROUND AS THE CPC. For this reason I recommend using the same 5V power supply as the CPC, with a Y cable.

You must not use this adapter on other host systems than the CPC line : on the Amstrad CPC the second button is connected on pin 7 which is 5V on Amiga/Atari port, and pin 9 is the secondary return pin instead of the second button. This system also has two "common" pins that are not connected to ground !

The adapter's software itself supports hot-plug both on DB9 and USB port, however there is no hardware protection. The manuals of the host systems warned to always turn it off before un/plugging any device. Again, you do it at your own risk.

# BOM
- 1x STM32F205RCT6
- 1x 3.3v LDO, either SPX3819M5-L-3-3 or XC6206P332MR
- 2x 1uF (or more) 0805 capacitors
- 1x 250mA polyfuse
- 2x 2.2uF 0805 capacitors
- 5x 100nF 0805 capacitors
- 4x 22ohm 0805 resistors
- 1x 4.7K 0805 resistor
- 3x 10K 0603x4 resistors array
- 2x USB A Female Socket, type G54
- 1x DB9 female sockets, 1x DB9 male socket ; I recommend using the connectors from a "Genesis gamepad extension cord" instead to avoid metallic shielding that can make shorts when connecting live.
- 1x barrel jack connector, 5.5x2.5 or 5.5x2.1 depending on your PSU (the first one is used on CPC+ and the second one on classic CPC)
- 1x Self-locking 5.8mm switch, not used yet
- D1 led and R3 are optional.

# Making it
Components are SMD, and have thin pin pitch. You need to know what you are doing.

For the DB9 plugs I personally use an extension cable cut in half, but you might solder a standard plug with the PCB board sandwiched between the pins.
Check for shorts at least between 5V, 3.3V, and GND traces before applying power !

The programming port does not need to be soldered since it is be programmed just once : you can just hold it in place during the few seconds required for programming.

There are several methods to program the STM32. I personally use a stlink clone with the open-source tools : https://github.com/stlink-org/stlink

# Using it
- Plug the DB9 cable
- Plug the USB controllers
- Plug the 5V barrel jack
- Switch power and CPC on

To keep things simple to use, each port has a preferred USB input : Joystick 1 with USB A and 2 with USB B. Except the mouse that must be connected on port B.
Here is more information about the USB devices you can connect.

## USB Mouse
Supports USB1 or USB2 mouse in boot mode, and emulates an AMX mouse.
Mouse must be connected on USB B and movements are sent on port 1.

## USB Keyboard
Supports USB keyboard in boot mode, and emulates at the same time two controllers on both ports.
Joystick 1 : AQOP or AQRT + Space or left Alt + Enter or Y
Joystick 2 : Keypad 7456 + 0 or 1 + Enter or '+'
The layouts are exchanged when plugged in the USB B port.

## XBOX360 and PS3 gamepad
Supports wired USB original and clones, and emulates a joystick, mouse, or dual pads depending on the active mode.

The round button and leds are used as a mode selector : press the round button to change mode.
5 modes are implemented by default, a few others are available in the source code, and you can make your own since it is open source :).

- gamepad + mouse = Standard game mode
1=Game : left stick and cross, autofire on right trigger
1=Mouse : right stick, left button is right stick hat and right button is back, autofire on left trigger

- gamepad + mouse = Port 2 mode
2=Game : left stick and cross, autofire on right trigger
2=Mouse : right stick, left button is right stick hat and right button is back, autofire on left trigger

- dual gamepads
Have you tried playing alone dual player games ?
1=Game : left stick and cross, autofire on right trigger
2=Game : right stick, left button is right stick hat and right button is back, autofire on left trigger

- mouse mode
For racing and platform titles when you prefer to use a button for jump or accelerate. And also for the case when you need a mouse on port 1.
1=Mouse : left/right on left stick and cross, autofire on right trigger
2=Game : right stick, left button is right stick hat and right button is back

- 2 players on one controller mode
To play with a friend when you are short on controllers, and are not afraid of a stiff neck.
1=Game : left part of the controller, 90° clockwise, autofire on left trigger
2=Game : right part of the controller, 90° counterclockwise, autofire on right trigger

## Other gamepad
May work with standard HID gamepads and emulates joystick on left stick, and mouse on right stick.
Since they are not normalized inside the USB documentation, right stick and buttons position my vary between controllers. Some even have strange things in their report descriptor which may also break their compatibility. Others need a specific setting of an analog/digital button.

