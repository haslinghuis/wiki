# KakuteF7

## Description

`KakuteF7 is a newer generation All-In-One flight controller of Holybro. It integrates flight controller (FC), power distribution board (PDB), and on-screen display (OSD) in one.`

## Benefits of KakuteF7 Compared to KakuteF4

`•F7 is a faster processor (216MHz vs 168MHz of F4), the faster speed F7 processor allows for faster looptime in theory without overclocking like we do with F4 running 32KHz`

`•F7 offers more UART’s with built-in hardware inversion`

`•F7 has superscalar pipeline and DSP capabilities – basically that means the F7 is a better platform for future development that allows the developers to further optimize the flight controller algorithms`

`•Onboard Baro BMP280, Support more flight mode`

`•Support SD card for Blackbox logging`

`•I2C1 Pads for external compass connection`

## Image

Top View
![](https://github.com/jamming/image/blob/master/KakuteF7-top-90.png?raw=true)

Bottom View
![](https://github.com/jamming/image/blob/master/KakuteF7-bottom-90.png?raw=true)

## Specifications

`•MCU: STM32F745VG 32-bit processor,216MHz`

`•IMU: ICM20689 and MPU6000 double IMU`

`•Baro: BMP280`

`•USB VCP Driver (all UARTs usable simultaneously; USB does not take up a UART)`

`•6 hardware UARTS (UART1,2,3,4,6,7)`

`•1 I2C for external compass`

`•SD card for Blackbox logging`

`•Dimensions: 35x43x6mm (includes USB in height)`

`•Mounting Holes: Standard 30.5mm square to center of holes `

`•Weight: 8g`

## Pinout Diagram

`Top View`
![]( https://github.com/jamming/image/blob/master/KakuteF7-top.png?raw=true)

`BUZ- : Piezo buzzer negative leg`

`LED       : WS2182 addressable LED signal wire`

`R1, T1    : UART1 RX and TX`

`R2, T2    : UART2 RX and TX`

`R3, T3    : UART3 RX and TX`

`R4, T4    : UART4 RX and TX`

`R6, T6    : UART6 RX and TX`

`R7        : UART7 RX`

`RSSI      : Analog (0-3.3v) RSSI input`

`PPM       : PPM Receiver input`

`SCL,SDA   : I2C1 signal`

`3V3       : 3.3v output (200 mA max)`

`5V        : 5v output (1.5 A max)`

`M1 to M6  : Motor signal outputs`

`VO        : Video output to video transmitter`

`VI        : Video input from FPV camera`

`Boot      : Bootloader button`

`G         : Ground`

`B+        : Battery positive voltage (2S-6S)`

`+         : Main battery lead positive`

`-         : Main battery lead negative`

## Target Code   
`KAKUTEF7`

## Manufacturers and Distributors

 www.holybro.com (Manufacturer & Designer)

Distributors:

## FAQ & Known Issues

`“Board Align” Feature Documentation`

## Other Resources

`Contact us at:`

`•Email: productservice@holybro.com`

`•Facebook Page: Holybro`

`•Facebook Group: Holybro Hobby Official Group`
