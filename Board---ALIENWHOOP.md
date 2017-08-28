## ALIENWHOOP Board

![IMAGE AlienWhoop F7 Flight Controller Welcome](https://scontent-iad3-1.xx.fbcdn.net/v/t1.0-9/20375760_1452516294794035_3452827763551447574_n.png?oh=ce87191ab286e5735aee62b3992636ee&oe=5A38B747)

AlienWhoop flight controller for Tiny Whoop, Blade Inductrix, Eachine, BetaFPV, and other micro brushed quadcopter frames. Best in class flight controller running BetaFlight 3.2 release candidate (upgradable).

* Choice of the top performing ARM processors: 
  * High-performance ST Microelectronics ARM Cortex-M4 core F4 168 MHz CPU
  * High-performance ST Microelectronics ARM Cortex-M7 core F7 216 MHz CPU
  * Highest-performer ST Microelectronics ARM Cortex-M4 core F4 *OVERCLOCKED to 240 MHz!!*
* Choice of top performing motion processors: 
  * Invensense MPU-6500 Six-Axis (Gyro + Accelerometer) low power consumption MEMS MotionTracking™ Device
  * Invensense MPU-9250 Nine-Axis (Gyro + Accelerometer + Compass) low power consumption MEMS MotionTracking™ Device
* Latest BetaFlight firmware running the ALIENWHOOPF4 or ALIENWHOOPF7 target. Capable of 32kHz gyro sampling and 32kHz PID loop with overclocked F4 in BetaFlight 3.2 (*32kHz gyro sampling rate might not be optimal depending on features selected 16/16 or 16/8 might be preferable*).
* Extreme power. Choice of either (1) Fairchild Semiconductor FDMA410NZ MOSFET with 9.5A continuous and 24A burst brushed motor insanity (8.5mm coreless scream), (2) FDMA410NZT MOSFET with 9.5A continuous and 63A burst, or (3) *Infineon Technologies IRFHS8342 MOSFET up to 75A pulsed!!*
* UART4 solder pads for programmable LED strip (SUPER COOL) using WS2812B RGB or RX/TX for Micro MinimOSD
* Choice of external receivers. Officially supporting FrSky XM and XM+ (SBUS), LemonRX DSM2 and DSMX (SBUS), and FlySky FS-A8S (iBUS) satellites.

## MCU, Sensors and Features

## Hardware

  - MCU: STM32F405RGT6 or STM32F722RET6
  - Brushed Motor Mosfets: Fairchild Semiconductor FDMA410NZ (9.5A/23A pulsed), FDMA410NZT (9.5A/63A pulsed), or Infineon IRFHS8342 MOSFET up to 75A pulsed
  - IMU: MPU-6500 or MPU-9250 (both SPI)
  - BARO: N/A
  - USB: STM32 VCP 
  - Hardware UARTS: UART4 solder pads for Micro MinimOSD, LED strip, etc.
  - OSD: N/A
  - Blackbox: N/A
  - PPM/UART Shared: N/A 
  - Battery Voltage Sensor: N/A
  - Current sensor: N/A
  - Integrated Voltage Regulator: 3.3V
  - Buttons: 2 jumpers (1: DFU, 2:Receiver Bind)

## Manufacturers and Distributors
  AlienWhoop (Manufacturers)
  
  - Prebuild boards here: https://groupgets.com/campaigns/297-alienwhoop-v2-flight-controller
  - DIY files here: https://oshpark.com/projects/m61Bc99Q

## Designers

  AlienWhoop

## Maintainers

  [@brucesdad13](https://github.com/brucesdad13)

## Acknowledgements

*Acknowledgements: AlienWhoop V2 is a remix of [AlienFlight F3 Quad Brushed V1](https://github.com/brucesdad13/AlienFlightArchive/tree/master/Flight-Controllers/F3-V1/F3-Quad)