_(This is a Basic Template to provide an outline for board info. Copy/paste this into a new board page and fill in, add or delete as needed)_

# KIWI F4



## Description



## MCU, Sensors and Features

### Hardware
_(Fill in hardware specs and add any not listed)_
  - MCU: 
  - IMU: 
  - IMU Interrupt: 
  - BARO: 
  - VCP: 
  - Hardware UARTS: 
  - OSD: 
  - Blackbox: 
  - PPM/UART Shared: 
  - Battery Voltage Sensor: 
  - Integrated Voltage Regulator: 
  - Brushed Motor Mosfets: 
  - Buttons: 

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
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

_format is reporter [name], (status): issue contents_

###Troubles Entering Bootloader Mode (DFU):
Some devices (e.g. receivers connected to SBUS/IBUS port or devices connected to one of the UARTS) can inhibit the FC from entering USB bootloader mode. In this case the FC will not be detected by Windows/MacOS. Windows detects the FC as "Unknown Device", MacOS reports "enumeration errors". If you see some of these errors unplug all devices from the FC and flash the FC standalone.

###Voltage and Current Scaling:  
Flying Lemon said to use the following for scale:
voltage 57, current 320.  

## Other Resources

Setup Guide: 

Rcgroups Thread: 

## Image