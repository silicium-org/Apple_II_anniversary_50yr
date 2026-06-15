# A keyboard interposer board for USB remote control of the Apple //e

## Apple //e hardware
J17 keyboard connector is IDC 0.1" 2x13 pins
- 8 outputs Y0..7 (from motherboard to keyboard)
- 10 inputx X0..9 (from keyboard to motherboard)
- 6 open/ground switches for the modifier keys (shift, control, reset, caps lock, open apple, closed apple)
- +5V and GND

J16 numeric keypad connector is a 1x11 0.1" connector with 6 of the Y lines and 4 of the X lines, the same ones as the main keyboard, with different combinations of X and Y closures.

## Analysis
- Because the keyboard is entirely a passive matrix of open/gnd switches, the interposer board can simply use gnd/open outputs (GPIO active-low with pull-up) in parallel of the keyboard. There is no need for separate inputs from keyboard to microcontrollers and outputs from microcontroller to motherboard.
- Numeric keypad J16 connector uses the same X and Y lines -> interposer board has numeric keypad support for free, no need to connect J16.

KKR75 has STM32F423 (TBC) in stock.

The II and II+ have a completely different design with an "intelligent" keyboard that has active logic for scanning the keyboard matrix, and sending a 7-bit parallel ASCII code to the motherboard => too different, our interposer board shall support only the //e and //e enhanced keyboards.

## Specification
- keyboard interposer function for //e only: 
  - microcontroller reads Y0..7 in parallel of the keyboard, so that it knows which row is being polled
  - microcontroller drives X0..9 to GND when it senses USB HID keyboard events
  - if by chance the user presses the real keyboard, the worst that can happen is ghost keypresses, exactly like the N-key rollover problem of PC gamers.

- base specification
  - the STM32 USB host port is connected to one Type-A connector, user uses an extension cable or just routes the keyboard wire inside the //e.
  - the PCB shall be cut in the shape of an Apple apple.
  - the PCB could be mechanically held in place solely by an IDC-26 female connector on the underside, with adequate clearance between the side of the //e case and the speaker connector (there seems to be about 8x8cm TBC)

- optional features
  - a 3- or 4-port USB hub
  - an IDC 2x5 outputting back to the joystick DE-9 (joystick emulation from USB-HID gamepads). Requires 6 GPIOs.
  - a 2nd IDC 2x5 outputting back to the Mouse Card DE-9 (mouse emulation from USB-HID mouses). Requires 6 GPIOs.
  - optionally, an I2C link to Appletini so that Appletini emulates the Mouse Card from a USB mouse connected to our keyboard interposer card. 2 GPIOs.

  - alternative to use in combination with Appletini, could we have the 50-pin edge card so that the interposer board would also emulate the Mouse Card
  
    !!! IT MAY BE UNREALISTIC TO BIT-BANG THE 6502 BUS FROM AN STM32 (a 200MHz ARM Cortex-M4 is totally capable of driving the 1MHz bus at 200ns timing resolution, but this requires almost 50 GPIO, or about 20 GPIO with chipselect address decoding in 74xx logic, and it makes it much more difficult to interleave the rest of the software, and USB interrupts may be problematic for software timings) !!!

    - if we do this Mouse Card emulation, the interposer board shall still work without being inserted in an expansion slot, in case the //e is already full
      - this requires arbitrating between IDC-26 female connector for piggy-backing to the motherboard, or male connector for a short ribbon cable when the interposer cable is stand-alone in the front of the //e under the keyboard

## Preliminary design
- STM32 for USB host + many 5V-compatible GPIOs + easy firmware upgrade
  - TBC add a micro-B connector (or Type-C) to the USB Device port of the STM32 for DFU when a BOOT0 microswitch is pressed
  - TBC do STM32 still need this or could we just have DFU when the micro-B is connected and powering the board?
  - BE SURE TO NOT DAMAGE the STM32 or //e motherboard irrespective of which of the micro-B connector or IDC-26 connector is powering +5V to the STM32
