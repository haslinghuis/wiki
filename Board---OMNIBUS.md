# OMNIBUS

_Omnibus is Latin for "for all"._

## Description

The Omnibus F3 flight controller is an integrated flight controller and OSD specifically designed for ease of use and outstanding flight performance. The SPI-connected MPU6000 inertial motion sensor was chosen for it's high reliability, accuracy and update speed. This board has no problem running fast loop times and ESC protocols. There is an onboard barometer for altitude sensing along with an On Screen Display (OSD) chip directly connected to the main processor (MCU). This tight integration between the MCU and the OSD enables fast updates to the display and easy configuration of the OSD, which is managed straight from the BetaFlight configuration tool. You no longer need to worry about the extra hassle of configuring your OSD with a USB/UART adapter and 3rd party configuration tool, it's all built into the flight control software.

For maximum ease of use, the OmnibusF3 has an onboard voltage regulator that can easily handle up to a 5s battery. No need to mess with a PDB, just plug your battery straight into the flight controller and you're ready to go! Along with the robustly engineered power management system on the OmnibusF3, special precautions have been taken to ensure that the sensitive OSD chip is well protected, so you don't have to worry about any problems with your OSD. No need to use a spark arrestor, even on 5s!

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F3
  - IMU: MPU6000 (SPI)
  - IMU Interrupt: Yes
  - BARO: BMP280 (SPI)
  - VCP: Yes
  - Hardware UARTS: 3
  - OSD: Yes, BetaFlight OSD (BFOSD)
  - Blackbox: SD Card
  - PPM/UART Shared: UART3 (optionally)
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary
  - Integrated Voltage Regulator: Yes, supports up to 5S
  - Brushed Motor Mosfets: No
  - Buttons: 2 (1: DFU, 2: unassigned)

### Features
  - Current Sensor: PA1
  - BlHeli passthrough: Yes
  - WS2811 Led Strip: Yes
  - Transponder: Yes
  - Beeper: Inverted

## Manufacturers and Distributors

[Airbot](https://myairbot.com)

Available here: http://shop.myairbot.com/index.php/flight-control/cleanflight-baseflight/omnibusv11.html

## Designers

[Airbot](https://myairbot.com) and [Nathan](https://github.com/nathantsoi)

## Maintainers
_(add your name here if you help test or contribute code for this board)_

[Nathan](https://github.com/nathantsoi)

[QuadroBro](https://github.com/QuadroBro)

## Similar Targets

_(add links board descriptions here that are similar in features or function, but have a separate target)_

- [SirinFPV](/betaflight/betaflight/wiki/Board---SIRINFPV)

## Variants

OMNIBUS AIO F3 PRO - http://shop.myairbot.com/index.php/omnibus-prov1-72.html

Diffrences:
  - Added Current Sensor
  - Added Power Filter
  - SBEC instead of LDO

## FAQ & Known Issues
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

_format is reporter [name], (status): issue contents_


## Other Resources

Setup Guide: https://nathan.vertile.com/blog/2016/07/07/omnibus-typhoon-miniquad/

Rcgroups Thread: http://www.rcgroups.com/forums/showthread.php?t=2711617

## Image
OMNIBUS AIO F3
![](http://shop.myairbot.com/media/catalog/product/cache/1/image/54b2359dd2430bcca06ee462d488eb40/o/m/omnibusf3-v1.1-3.jpg)
OMNIBUS AIO F3 PRO
![](http://shop.myairbot.com/media/catalog/product/cache/1/image/54b2359dd2430bcca06ee462d488eb40/o/m/omnibusf3-pro-4_1.jpg)
