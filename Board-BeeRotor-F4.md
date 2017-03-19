# BeeRotor F4

![BeeRotor F4 front](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/beerotorf4/beerotorf4_front.jpg)

![BeeRotor F4 back](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/boards/beerotorf4/beerotorf4_back.jpg)

## Description

F4 board with integrated Betaflight OSD.

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F405
  - IMU: MPU6050A (SPI) 
  - IMU Interrupt: yes
  - BARO: BMP280 (I2C)
  - VCP: yes
  - Hardware UARTS: 1, 2, 3
  - OSD: Betaflight OSD
  - Blackbox: serial / SD Card
  - PPM/UART Shared: UART2
  - Battery Voltage Sensor: yes
  - Integrated Voltage Regulator: no
  - Brushed Motor Mosfets: no
  - Buttons: BOOT
  - 8 PWM / DShot outputs (up to 6 useable for DShot)
  - LED strip output
  - IR transmitter output
  - switchable inverters for UART2 (SBus RX) and UART3 (SmartPort telemetry)
  - SPI connector

### Features

  - 8 motor outputs (6 useable for DShot)
  - integrated Betaflight OSD
  - blackbox logging to SD Card

## Manufacturers and Distributors

RCTimer: http://rctimer.com/product-1730.html

## Designers

Eric Liang (imericliang@gmail.com)
Michael Keller

## Maintainers

Michael Keller

## FAQ & Known Issues

- Enabling DShot for motor 6: https://github.com/betaflight/betaflight/wiki/DSHOT-ESC-Protocol#beerotor-f4----for-use-with-a-hexacopter-motor-6-needs-to-be-moved-to-the-led-pin-on-the-sc-connector