# KakuteF4-AIO

## Description

`KakuteF4-AIO is a fantastic and popular flight controller designed for FPV racing drone DIY. It is designed around the STM32F4 MCU and ICM20689 IMU.The board size  is 35x44.5mm, and the mounting holes is 30.5x30.5mm.`

## Image of Top and Bottom

![](https://github.com/jamming/image/blob/master/KakuteF4-AIO-top.jpg?raw=true)

![](https://github.com/jamming/image/blob/master/KakuteF4-AIO-bottom.jpg?raw=true)

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F405RGT6
  - IMU: ICM20689 (SPI)
  - OSD: MAX7456
  - SPI Flash: MX25L25635FMI
  
### Features
  - The ICM20689 is the new gyro designed by Invensense, it’s low noise floor and high reliability rate makes it a   top choice for FPV flight controller.
  - F4 processor runs at 168MHz, it allows you to run high loop times.
  - Onboard 32MBytes SPI Flash for storage of flight logging.
  - FrSky Telemetry support for S.Port style receivers.
  - Power distribution, 120A capability.
  - One analog input for RSSI monitoring.
  - BlHeli passthrough: YES.
  - WS2811 Led Strip: implemented.
  - VCP: Yes.
  - Hardware UARTS: 3,Capable of Telemetry, GPS, DSM, Debug, MavLink, HOTT, FrSky Sensor.
  - 6 Servo outputs, capable of traditional PWM or OneShot.
  - OSD: MAX7456,directly configured by betaflight-configurator.
  - PPM/UART Shared: UART6
  - Battery Voltage Sensor: Yes, onboard, no wiring necessary
  - Battery Current Sensor: Yes, onboard, no wiring necessary
  - Integrated Voltage Regulator: Yes, 7v~42v input,5V/1.25A output
  - Buttons: 1 - DFU
  - CAN Bus which is very useful for talking to external devices.

## Hardware Designs (if available)

`The hardware is currently closed source.`

## Manufacturers and Distributors

`www.holybro.com`


## FAQ & Known Issues
Here is an example wiring  for FPV drone: 

![](https://github.com/jamming/image/blob/master/KakuteF4-AIO-Wire.jpg?raw=true)

## Other Resources
_(add setup guides and instructional material here)_

Rcgroups Thread: `link goes here`