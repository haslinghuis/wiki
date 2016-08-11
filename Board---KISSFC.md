# KISSFC

_Keep it simple stupid._

## Description

KISSFC is a STM32F3 based flight controller with integrated voltage regulator, brushed motor drivers and a shared PPM/UART RX input pin.

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F3
  - IMU: MPU6050 (SPI)
  - IMU Interrupt: Yes
  - BARO: No
  - VCP: Yes
  - Hardware UARTS: 3
  - OSD: No
  - Blackbox: No
  - PPM/UART Shared: UART2
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary
  - Integrated Voltage Regulator: Yes, unknown voltage limit
  - Brushed Motor Mosfets: Yes
  - Buttons: 1 - DFU

### Features
  - Current Sensor: Not implemented
  - BlHeli passthrough: No (due to buffered outputs)
  - WS2811 Led Strip: Not implemented
  - Transponder: No

## Hardware Designs (if available)

## Manufacturers and Distributors

[Flyduino.net](https://flyduino.net)

Available here: http://flyduino.net/KISS-FC-32bit-Flight-Controller-V103_1

## Designers
_(add your name here if you conributed to the design of this board)_

[fedorcomander](https://github.com/fedorcomander)

## Maintainers
_(add your name here if you help test or contribute code for this board)_

## Similar Targets
_(add links board descriptions here that are similar in features or function, but have a separate target)_

## Variants
_(add links to boards here that are similar in features or function, but use this target when flashing)_

## FAQ & Known Issues
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

## Other Resources

Pinouts, schematics and RX wiring: http://nathan.vertile.com/blog/2016/07/29/betaflight-kiss-flight-controller/#pinout

Rcgroups Thread: http://www.rcgroups.com/forums/showthread.php?t=2555204

## Image

![](http://flyduino.net/bilder/produkte/gross/KISS-FC-32bit-Flight-Controller-V103.jpg)