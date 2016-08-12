# BLUEJAYF4 (including mini)

Beautifully simple STM32F4 based flightcontroller. An F4 replacement for the Naze.

![BlueJayF4 - rev3](https://cloud.githubusercontent.com/assets/6168871/17614562/ac35c8ee-60aa-11e6-8fd1-6457ee934784.jpg)

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

## Manufacturers and Distributors

These boards (full and mini) are currently being manufactured in limited batches and is available at some sites, and directly from the manufacturers.  

Available here: [BlueJayRC.com](https://bluejayrc.com)

## Hardware Designs (if available)

The hardware is currently closed source. It may be in the future that these will be made available.

## Variants

BlueJayF4 rev1, 2 and 3 - including mini.

Rev3 (mini) - including accompanying PDB
![bluejayf4-mini](https://cloud.githubusercontent.com/assets/6168871/17614560/ac145f92-60aa-11e6-8e4b-c448164bb988.jpg)

Rev3 (Full Size)
![BlueJayF4 - rev3](https://cloud.githubusercontent.com/assets/6168871/17614562/ac35c8ee-60aa-11e6-8fd1-6457ee934784.jpg)

![BlueJayF4 - rev3 - bottom](https://cloud.githubusercontent.com/assets/6168871/17614561/ac33b9e6-60aa-11e6-8d12-87a02691a7c7.jpg)

Rev2 (Full Size)
![BlueJayF4 - rev2](https://cloud.githubusercontent.com/assets/6168871/17614346/01cef75a-60a9-11e6-93f9-16248d6def11.jpg)
## FAQ & Known Issues

Serial Wire Debug output is located on the bottom of the board, and provides a pin out compatible with STM32Fx discovery boards to be used as a SWD adapter:
![bjf4-swd](https://cloud.githubusercontent.com/assets/6168871/17614348/066cf4d8-60a9-11e6-89f3-c439c5654ba2.jpg)

Known Issues:
The rev2 requires a resistor mod to prevent the issue of crashing on power up. The rev3 does not have this issue.

Following the picture below to perform the Rev2 resistor modification:
![resistor-mod](https://cloud.githubusercontent.com/assets/6168871/17614652/3daa257c-60ab-11e6-8567-ab51625e8e89.png)

The rev2 onboard regulator is limited in current capacity, and has been replaced with a pololu piggy back option for greater flexibility.
![soldered-pololu](https://cloud.githubusercontent.com/assets/6168871/17614559/abe4d650-60aa-11e6-8c85-93ed35a8b04f.jpg)

## Other Resources

Rcgroups Thread: http://www.rcgroups.com/forums/showthread.php?t=2593106