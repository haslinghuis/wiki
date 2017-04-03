# KIWI F4 V2


## Description

KIWI V2 is a rebulided V1, added s.port inverter, sdcard, redesigned PCB

## MCU, Sensors and Features

### Hardware
_(Fill in hardware specs and add any not listed)_
  - MCU: STM32F405RGT6
  - IMU: MPU6000
  - IMU Interrupt: 
  - BARO: no
  - VCP: yes
  - Hardware UARTS: 4
  - OSD: yes 
  - Blackbox: SDCard
  - PPM/UART Shared: 
  - Battery Voltage Sensor: yes
  - Integrated Voltage Regulator: yes 
  - Brushed Motor Mosfets: no
  - Buttons: BOOT

### Features

_(add list of features)_

## Manufacturers and Distributors

[https://flyinglemon.eu/flight-controllers/39-kiwif4-flight-controller.html](https://flyinglemon.eu/flight-controllers/39-kiwif4-flight-controller.html)
[https://beaverfpv.com/collections/new-arrivals/products/kiwi-f4-flight-controller-kiwi-pdb](https://beaverfpv.com/collections/new-arrivals/products/kiwi-f4-flight-controller-kiwi-pdb)

## Designers
* JohnLemon
* Flyinglemon

## Maintainers
_(add your name here if you help test or contribute code for this board)_


## Similar Targets

_(add links board descriptions here that are similar in features or function, but have a separate target)_


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
 
[http://flyinglemon.eu/ext_images/kiwif4V2_wiring_diagram.pdf](http://flyinglemon.eu/ext_images/kiwif4V2_wiring_diagram.pdf)

Rcgroups Thread: 

## Image

![](http://flyinglemon.eu/ext_images/KIWIV2_TOP2_S.jpg)
![](http://flyinglemon.eu/ext_images/KIWI_COMBO_S.jpg)