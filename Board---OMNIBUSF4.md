# OMNIBUS F4, F4 V2 and F4 Pro


These boards are called differently depending on the distributor.
```
+----------------------+----------------------------+-------------+
| myairbot.com         | RTFQ                       | BF target   |
+----------------------+----------------------------+-------------+
| OMNIBUS AIO F4       | FLIP32-F4-OMNIBUS          | OMNIBUSF4   |
| OMNIBUS AIO F4 V2    | FLIP32-F4-OMNIBUS V2       | OMNIBUSF4SD |
| OMNIBUS F4 Pro       | FLIP32-F4-OMNIBUS V2 PRO   | OMNIBUSF4SD |
| OMNIBUS F4 Pro (v2)  |                            |             |
+----------------------+----------------------------+-------------+
```
## OMNIBUS F4 AIO Features

- SPI Gyro MPU6000
- STM32 F405 MCU, Runs Betaflight 3.0 firmware
- SBUS/PPM input (Pinheaders)
- 6PWM output (1-4Pinheaders and Sh1.0 Plug, 5-6 as Pinheaders)
- NO BARO
- 128Mbit Flash

## OMNIBUS F4 Pro features

- SPI Gyro MPU6000
- On-Board OSD (controlled by Betaflight, FC over SPI bus)
- MicroSD Blackbox
- Baro (BMP280)
- F4 Processor (F405)
- 5v 3a SBEC
- Built-in Current Sensor
- On-Board Video Filter (can only supply 5V to VTX and Camera)

## Betaflight 3.1 specific configuration
### OMNIBUS F4 AIO

- Use OMNIBUSF4 target.
- LED strip port is PWM5.

#### Resource mapping (WIP)

| Label    | Pin | Timer | DMA | Note                             |
|----------|-----|-------|-----|----------------------------------|
| RSSI     | A0  |       |     | Not ADC capable                  |
| PWM5     | A1  | 5,2   | ?,? |                                  |
| PWM4     | A2  | 2,3   | 1,1 |                                  |
| PWM3     | A3  | 2,4   | 1,6 |                                  |
| PWM6     | A8  | 1,1   | ?,? |                                  |
| UART1 TX | A9  | ?,?   | ?,? |                                  |
| UART1 RX | A10 | ?,?   | ?,? |                                  |
| PWM1     | B0  | 3,3   | 1,7 |                                  |
| PWM2     | B1  | 3,4   | 1,2 |                                  |
| LED      | B6  | 4,1   | ?,? |                                  |
| PPM      | B8  | 12,3  | ?,? |                                  |
| CH2      | B9  | 12,4  | ?,? |                                  |
| UART3 TX | B10 | ?,?   | ?,? |                                  |
| UART3 RX | B11 | ?,?   | ?,? |                                  |
| CRNT     | C1  | ?,?   | ?,? |                                  |
| VBAT     | C2  | ?,?   | ?,? |                                  |
| UART6 TX | C6  | ?,?   | ?,? |                                  |
| UART6 RX | C7  | ?,?   | ?,? |                                  |
| CH5      | C8  | 8,3   | ?,? |                                  |
| CH6      | C9  | 8,4   | ?,? |                                  |

### OMNIBUS F4 V2 and F4 Pro

- Use OMNIBUSF4SD target.
- LED strip port is PWM6.

#### Resource Mapping (WIP)

(Not available yet)

## Other Resources

RCgroups thread for the F4 AIO at <https://www.rcgroups.com/forums/showthread.php?2754926-Omnibus-F4-AIO>

RCgroups thread for the F4 Pro at <https://www.rcgroups.com/forums/showthread.php?2801694-Omnibus-f4-pro>

Layouts for F4 Pro [Top](https://www.rcgroups.com/forums/showatt.php?attachmentid=9631520&d=1482680395) [Bottom](https://www.rcgroups.com/forums/showatt.php?attachmentid=9631521&d=1482680397)