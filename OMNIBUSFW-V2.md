# OMNIBUS Fireworks v2

[Flight Controller](https://imgur.com/BpeSasH)

## Description

The Omnibus Fireworks flight controller uses the ICM20608
over SPI mounted inside of an onboard damping box.
Also on-board is a barometer and AB7456 OSD chip for the BetaFlight integrated OSD.

Omnibus Fireworks supports 3-6s LIPO direct input, build in hall Current Sensor and Power Filter too.

## MCU, Sensors and Features

### Hardware

| Hardware      | Part Number   | Notes|
|---------------|---------------|------|
| MCU  | [STM32F405RGT6](http://www.mouser.com/ds/2/389/DM00037051-492832.pdf)  | 4 Hardware UARTS - Shared PPM/UART TBD|
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





## FAQ & Known Issues

### Troubles Entering Bootloader Mode (DFU):
Some devices (e.g. receivers connected to SBUS/IBUS port or devices connected to one of the UARTS) can inhibit the FC from entering USB bootloader mode. In this case the FC will not be detected by Windows/MacOS. Windows detects the FC as "Unknown Device", MacOS reports "enumeration errors". If you see some of these errors unplug all devices from the FC and flash the FC standalone.

### Voltage and Current Scaling:  
 


## Resource mapping

## Other Resources

### Enable Camera Control:

###Setup Guide:


[Pinout Top](https://imgur.com/6Tdb4jW)


[Pinout Bottom](https://imgur.com/DMmMzoI)

Dimensions:
FC: X(L) x X(W) x X(H)
