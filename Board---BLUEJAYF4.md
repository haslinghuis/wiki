# BLUEJAYF4 (including mini)

Beautifully simple STM32F4 based flightcontroller. An F4 replacement for the Naze.

## Description

Two variants (in multiple revisions). The full size, 36x36mm (30.5x30.5 mounting holes) and a 25x25mm (20x20 mounting holes) mini version.

## MCU, Sensors and Features

### Hardware
  - MCU: STM32F405RTG6
  - IMU: ICM-20608-G (SPI) rev3, and MPU9250 (SPI) rev1 and rev2.
  - IMU Interrupt: Yes
  - BARO: Optional on full size, not available on mini.
  - VCP: Yes
  - Hardware UARTS: 3 (4 on full size with Quad motor remapping)
  - OSD: No
  - Blackbox: Yes (16mb rev3, 2mb rev1), SD card for rev2 and rev3 full size (no SD card on mini). 
  - PPM/UART Shared: UART6
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary (if using pololu on full size), wiring required on mini
  - Integrated Voltage Regulator: Pololu piggy back option on full size rev3.
  - Brushed Motor Mosfets: No
  - Buttons: 1 - DFU

### Features
  - Current Sensor: Not implemented
  - BlHeli passthrough: Yes 
  - WS2811 Led Strip: Yes (on motor output Pin 6)
  - Transponder: No

## Hardware Designs (if available)

The hardware is currently closed source. It may be in the future that these will be made available.

## Variants

BlueJayF4 rev1, 2 and 3 - including mini.

## FAQ & Known Issues

The rev2 requires a resistor mod to prevent the issue of crashing on power up. The rev3 does not have this issue.
The rev2 onboard regulator is limited in current capacity, and has been replaced with a pololu piggy back option for greater flexibility.

## Other Resources

Rcgroups Thread: http://www.rcgroups.com/forums/showthread.php?t=2593106