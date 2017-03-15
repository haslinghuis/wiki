# Demon Soul F4


## Description
Derive from REVO, has inverters for SBUS and SPORT, only one full-fledged UART3 broken out.


## MCU, Sensors and Features

### Hardware
_(Fill in hardware specs and add any not listed)_
  - MCU: STM32F405
  - IMU: MPU-6000
  - IMU Interrupt: 
  - BARO: No
  - VCP: Yes
  - Hardware UARTS: UART1 for SBUS with Rx only, UART6 for SPORT, UART3 available for general use
  - OSD: No
  - Blackbox: SPI 2MB
  - PPM/UART Shared: Yes
  - Battery Voltage Sensor: Yes
  - Integrated Voltage Regulator: No
  - Brushed Motor Mosfets: No
  - Buttons: No

### Features

_(add list of features)_

## Manufacturers and Distributors

http://demonrc.eu/product/demon-soul-f4-high-performance-flight-controller/


## Designers

Adam Tusk (?)


## Maintainers

_(add your name here if you help test or contribute code for this board)_


## Similar Targets

REVO F4

## FAQ & Known Issues
* PB2/BOOT1 pin is not grounded on this board, seems to be floating, and hence board may refuse to go into DFU mode even with the boot pads shorted. This issue may me solved my running a small jumper wire from PB2 to GND.
* Board has SWD pins broken out on it's back with the following pinout: GND SWCLK SWD NRST VDD

## Image

![Front Face](http://demonrc.eu/wp-content/uploads/2017/01/Demon-Soul-F4-Flight-Controller-Connection-Diagram.jpg)
![Back Face](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/soulf4/soulf4-back-face.png)
