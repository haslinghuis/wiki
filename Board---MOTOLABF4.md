# Typhoon F4 Flight Controlle
## Description
Two board set


## Hardware
### Typhoon F4 Flight Controller:
 - 168MHz STM32F4 version of Tempest
 - 100+A PDB with motor current sensor
 - MPU6000 gyro/acc on SPI bus
 - Up to 4 serial ports plus USB
 - Up to 6 Dshot motor outputs
 - Serial inverters for SBUS and S.PORT
 - 1.5A 5V regulator with LC filtered 5V for VTX and camera
 - Plug-in connection to VTX board
 - 38x40mm board size with 30.5mm mounting hole spacing

 - The Typhoon F4 is compatible with Betaflight using the new MOTOLABF4 board target.
 - The F4 board mates directly with the VTX board with no external wiring between the two.

### Typhoon VTX:
 - 25/200/500mW switchable, 40-ch video transmitter
 - Betaflight OSD
 - MicroSD slot for Blackbox
 - Direct plug-in connection to camera and antenna with available custom cables
 - Pass-through connector for camera joystick
 - 38x38mm board with 30.5mm mounting hole spacing

 - The Typhoon VTX is a self-contained video system, operating on filtered 5V provided by the Typhoon F4. The video is isolated from the PDB power distribution and delivers cleaner video.

 - The VTX board mates directly with the F4 controller board with no external wiring between the two.

 - Note: The VTX board is not powered from the USB input. The board should NOT be powered from lipo without an antenna connected. 

The VTX controls are through the BetaFlight OSD functions using the RC transmitter. I think there may be documentation in the wiki. The VTX requires uploading a font through the Font Manager in BetaFlight Configurator. The only other control is the power selection switches. The settings for that are:

SW1 ON, SW 2 ON 25mW
SW1 ON, SW 2 OFF 200mW
SW1 OFF, SW2 OFF 500mW

Regarding soft mounting, that's never been necessary on any of my builds or those of the beta testers here. With the MPU6000, the well-filtered power in the Typhoon, and a stiff frame there shouldn't be a problem. The video above has the FC hard mounted.

### Software
  - Firmware target: MOTOLABF4

### Features

_(add list of features)_

## Manufacturers and Distributors

They'll be in stock at Heli-Nation, RocketCityFPV, DefianceRC and others.

## Designers
Moto Moto

## Maintainers
_(add your name here if you help test or contribute code for this board)_


## Similar Targets

[MotoLab Tempest](https://github.com/betaflight/betaflight/wiki/Board---MOTOLAB)


## Variants

Differences:


## FAQ & Known Issues


## Other Resources

Setup Guides: 
https://www.rcgroups.com/forums/showthread.php?2715556-MotoLab-Flight-Controllers

Pin Out diagram is attached in this post:  
https://www.rcgroups.com/forums/showpost.php?p=37498779&postcount=595


## Image
https://www.rcgroups.com/forums/showpost.php?p=37468289&postcount=3013
