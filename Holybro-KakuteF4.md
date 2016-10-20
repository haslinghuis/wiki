```
Instructions: remove or replace all code comments (backticks).
Follow italic directions and leave or remove when updated
Add your board to the list in the sidebar, double check the whole list if you want, with:
  find src/main/target/ -type d|xargs basename |tail -n +2
```

# Kakute F4

`Quality Flight Controller`

## Description
![](http://holybro-test-images.b0.upaiyun.com/201609/180/IMG_3436.JPG)
`what is special about this board?`

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F405RGT6
  - IMU: MPU9250 (SPI)
  - IMU Interrupt: Yes
  - BARO: No
  - VCP: Yes
  - Hardware UARTS: 3
  - OSD: No
  - Blackbox: Yes
  - PPM/UART Shared: UART6
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary
  - Integrated Voltage Regulator: Yes, 7v~42v
  - Brushed Motor Mosfets: No
  - Buttons: 1 - DFU

### Features

  - Current Sensor: Not implemented
  - BlHeli passthrough: No (due to buffered outputs)
  - WS2811 Led Strip: implemented
  - Transponder: No

## Hardware Designs (if available)

`schematics, etc.`

## Manufacturers and Distributors

`holybro.com`

Available here: `link to purchase`

## Designers
_(add your name here if you contributed to the design of this board)_

## Maintainers
_(add your name here if you help test or contribute code for this board)_

## Similar Targets
_(add links board descriptions here that are similar in features or function, but have a separate target)_

## Variants
_(add links to boards here that are similar in features or function, but use this target when flashing)_

## FAQ & Known Issues
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

## Other Resources
_(add setup guides and instructional material here)_

Rcgroups Thread: `link goes here`