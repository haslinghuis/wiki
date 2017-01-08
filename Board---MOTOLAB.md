#MotoLab 
## Description
Four FCs that all use the same Target HEX

- MotoF3- Board shape designed for the ImpulseRC Warp Quads as part of the frame.   
- Tornado- Does not allow BLHeli Pass-through due to have 5V uni-directional drivers on the motor output pins.  
  Requires a Pololu 5V regulator mounted on the board.
- Cyclone- Like the Tornado but supports BLHeli pass-through and has the 5V regulator built onto the board.  
- Tempest- Has PDB built into the board as well as a 5V regulator to power accessories.  


## MCU, Sensors and Features

### Hardware
  - MCU: STM32F3
  - IMU: ---
  - IMU Interrupt: 
  - BARO: No
  - VCP: Yes
  - Hardware UARTS: 3
  - OSD: No
  - Blackbox: Serial
  - PPM/UART Shared: UART2
  - Battery Voltage Sensor: Yes
  - Integrated Voltage Regulator: ---
  - Brushed Motor Mosfets: No
  - Buttons: None. Solder BOOT pads

### Features

_(add list of features)_

## Manufacturers and Distributors

_(add links to Manufacturers and Distributors)_

## Designers


## Maintainers
_(add your name here if you help test or contribute code for this board)_


## Similar Targets

_(add links board descriptions here that are similar in features or function, but have a separate target)_


## Variants

Differences:


## FAQ & Known Issues

To use DSHOT ESC protocol with ßF3.1 Motor1 needs to be re-mapped to the PPM pin. See the [DSHOT & Betaflight](https://github.com/betaflight/betaflight/wiki/BetaFlight%20and%20Dshot) page.  
A wire can be soldered from the PPM pin to the motor 1 header pin or just connect ESC#1 directly to the PPM pin.
Note: Adding this wire is not required if you connect signal wire from ESC #1 directly to the PPM pin.  

Photo of wire added to a Cyclone.
https://www.rcgroups.com/forums/showpost.php?p=36589146&postcount=2787

## Other Resources

Setup Guides: 

Tornado and Cyclone
https://www.rcgroups.com/forums/showthread.php?t=2537379  

Tempest
https://www.rcgroups.com/forums/showthread.php?t=2715556  

## Image

