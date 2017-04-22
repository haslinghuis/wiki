
## Description

Beautifully simple STM32F7 based flightcontroller. A F7 replacement for the Naze.

![NERO (TOP) - rev1](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/nero/nero-rev1-top.jpg)
![NERO (BOTTOM) - rev1](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/nero/nero-rev1-bottom.jpg)

## MCU, Sensors and Features

### Hardware
  - Size: 36x36mm (30.5x30.5 mounting holes)  
  - MCU: STM32F722RET6
  - IMU: ICM-20602 (SPI) 
  - IMU Interrupt: Yes
  - VCP: Yes
  - Hardware UARTS: 3 
  - OSD: Compatible pin-outs for MinimOSD on UART3 (stackable) 
  - Blackbox: SD card 
  - PPM/UART Shared: UART6
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary (if using pololu on full size)
  - Integrated Voltage Regulator: Pololu piggy back option 
  - Button for putting board into DFU mode

### Features
  - Current Sensor: available as ADC input, but requires shunt circuit on PDB or battery cable.
  - BlHeli passthrough: Yes 
  - WS2811 Led Strip: Yes (on motor output Pin 5)
  - Transponder: No

## Manufacturers and Distributors

These boards are currently available in pre-order only. Shipping is expected during May and June of 2017.  

Available here: [nerofc.com](https://nerofc.com)

## Configuration Information

### Wiring Diagrams
![Wiring Diagram - rev1](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/nero/nero-rev1-wiring.png)

### Schematics

The pin out for the MCU is provided here, so that others if they are thinking of developing a board using the same target so we don't need a multitude of targets going forward. I hope that it will also assist other developers in adding features.

![MCU Output Schematic - rev1](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/nero/nero-rev1-mcu-schematic.png)


# Other Resources

RC Groups Thread: https://www.rcgroups.com/forums/showthread.php?2734745-NERO-STM32F7-based-FC