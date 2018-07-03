# OMNIBUS Fireworks v2

![Flight Controller](https://image.ibb.co/gxmWGd/fireworksv2_1.jpg)

## Description

The Omnibus Fireworks flight controller uses the ICM20608
over SPI mounted inside of an onboard damping box.
Also on-board is a barometer and AB7456 OSD chip for the BetaFlight integrated OSD.

Omnibus Fireworks supports 3-6s LIPO direct input, contains a built-in hall effect Current Sensor, and provides on board power filtering.

## MCU, Sensors and Features

### Hardware

| Hardware      | Part Number   | Notes|
|---------------|---------------|------|
| MCU  | [STM32F405RGT6](http://www.mouser.com/ds/2/389/DM00037051-492832.pdf)  |  |
| IMU  | [ICM-20608](https://store.invensense.com/datasheets/invensense/ICM-20608-G-ProductSpec-V1.pdf)        | |
| OSD  | [AB7456](https://www.unmannedtechshop.co.uk/micro-osd-v2-3-ab7456/)     | Need actual datasheet |


| Features | Yes/No |
|----------|--------|
| Barometer | Yes |
| VCP | Yes |
| OSD | Yes |
| SD Card | No |
| Onboard flash | Yes |
| Voltage Sensor | Yes |
| Current Sensor | Yes|
| Boot Button | Yes| 



## Manufacturers and Distributors

[Airbot](https://store.myairbot.com/omnibusfireworksv2.html)


## Contributors

[MiddleMan5](https://github.com/MiddleMan5) - Documentation

## Variants

### Fireworks V1

    Changes since V1:
    * Footprints for (approx 8.9mm(L) x 4.2mm(W)) ESC output capacitors. Airbot does not provide the exact footprint
    * Solder pads for SmartAudio (UART2/GPIO PA2) and Camera Control (GPIO PB9) 
    * IMU reoriented
    * Ribbon cable fully contained within IMU cage
    * 8V@1A (buck) switching regulator and LC filter added for camera and VTX
    * Smartport uses software serial on GPIO PA9. Airbot claims this increases the quantity of UARTs to 5.
(More information will need to be provided on Betaflight's support of software serial on PA9)

## FAQ & Known Issues

### Troubles Entering Bootloader Mode (DFU):
Some devices (e.g. receivers connected to SBUS/IBUS port or devices connected to one of the UARTS) can inhibit the FC from entering USB bootloader mode. In this case the FC will not be detected by Windows/MacOS. Windows detects the FC as "Unknown Device", MacOS reports "enumeration errors". If you see some of these errors unplug all devices from the FC and flash the FC standalone.

## Voltage and Current Scaling:  

** From Betaflight 3.3 **

#### Voltage:
* Scale:      110
* Divider:     10
* Multiplier:   1

#### Current:
* Scale:   176
* Offset: -18500

## Resource mapping

## Other Resources

### Enable Camera Control:

### Setup Guide:


![Pinout Top](https://image.ibb.co/j9uq9y/Fire_Works_Pinout2_51557_1528920698.jpg)


![Pinout Bottom](https://image.ibb.co/jTZwhJ/Fire_Works_Pinout1_70404_1528920698.jpg)

Dimensions:
FC: X(L) x X(W) x X(H)
