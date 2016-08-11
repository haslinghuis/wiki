```
Instructions: remove or replace all code comments (backticks).
Follow italic directions and leave or remove when updated
Add your board to the list in the sidebar, double check the whole list if you want, with:
  find src/main/target/ -type d|xargs basename |tail -n +2
```

# TEMPLATE

`tagline`

## Description

`what is special about this board?`

## MCU, Sensors and Features

### Hardware
_(update to match the target)_
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
_(update to match the target)_
  - Current Sensor: Not implemented
  - BlHeli passthrough: No (due to buffered outputs)
  - WS2811 Led Strip: Not implemented
  - Transponder: No

## Hardware Designs (if available)

`schematics, etc.`

## Manufacturers and Distributors

`name of manufacturers and distributors for this specific board, variants go below`

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