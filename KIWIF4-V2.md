# KIWI F4 V2

![Flight Controller](http://flyinglemon.eu/ext_images/KIWIV2_TOP2_S.jpg)

## Description

MPU-6000 F4 flight controller with stackable PDB. Integrated 12V and 5V regulators. Built-in OSD, S.Port inverter, SD Card slot.

## MCU, Sensors and Features

### Hardware

| Hardware      | Part Number   | Notes|
|---------------|---------------|------|
| MCU  | STM32F405RGT6  | 4 Hardware UARTS - Shared PPM/UART TBD|
| IMU  | MPU6000        | Interrupt TBD |
| OSD  | MAX7456EUI     | |
| 12V Regulator | NCP1117 17-12G | LDO Linear: 1A Max |
| 5V Regulator | LMR14206 | Switching Freq: 1.25MHz |


| Features | Yes/No |
|----------|--------|
| Barometer | No |
| VCP | Yes |
| OSD | Yes |
| SD Card | Yes |
| Voltage Sensor | Yes |
| Current Sensor | Yes|
| Boot Button | Yes| 

## Manufacturers and Distributors

[Flying Lemon](https://flyinglemon.eu/flight-controllers/39-kiwif4-flight-controller.html)

[Beaver FPV](https://beaverfpv.com/collections/new-arrivals/products/kiwi-f4-flight-controller-kiwi-pdb)

## Designers
* JohnLemon
* Flyinglemon

## Maintainers
[FlyingLemonFPV](https://github.com/flyinglemonfpv)


## Similar Targets

[Kiwi F4](https://github.com/betaflight/betaflight/wiki/KIWIF4)

[Plum F4](https://github.com/betaflight/betaflight/wiki/Board---PLUMF4)

## Variants

Differences:


## FAQ & Known Issues
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

_format is reporter [name], (status): issue contents_

###Telemetry to FrSky XSR: 

###Troubles Entering Bootloader Mode (DFU):
Some devices (e.g. receivers connected to SBUS/IBUS port or devices connected to one of the UARTS) can inhibit the FC from entering USB bootloader mode. In this case the FC will not be detected by Windows/MacOS. Windows detects the FC as "Unknown Device", MacOS reports "enumeration errors". If you see some of these errors unplug all devices from the FC and flash the FC standalone.

###Voltage and Current Scaling:  
Flying Lemon said to use the following for scale:
voltage 57, current 444 offset 11.  

## Other Resources

Setup Guide:

![Wiring Diagram](https://i.imgur.com/WmDlIHV.jpg)

Dimensions:
FC: 36mm x 36mm x 6.8mm(H)
