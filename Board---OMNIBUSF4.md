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

| Label    | Pin | Timer | DMA | Default | Note                             |
|----------|-----|-------|-----|---------|----------------------------------|
| RSSI     | A0  |       |     |         | Not ADC capable                  |
| PWM5     | A1  | 5,2   | ?,? | motor 5                                 |
| PWM4     | A2  | 2,3   | 1,1 | motor 4 |                                  |
| PWM3     | A3  | 2,4   | 1,6 | motor 3   |                                  |
| PWM6     | A8  | 1,1   | ?,? | motor 6, led_strip                                  |
| UART1 TX | A9  | 1,2   | ?,? | serial_tx 1 |                                |
| UART1 RX | A10 | 1,?   | ?,? | serial_rx 1 |                              |
| PWM1     | B0  | 3,3   | 1,7 | motor 1 |                                  |
| PWM2     | B1  | 3,4   | 1,2 | motor 2 |                                  |
| LED      | B6  | 4,1   | ?,? | J9 (Not accessible with v3.1)    |
| PPM      | B14 | 12,3  | ?,? | ppm     |                                 |
| CH2      | B15 | 12,4  | ?,? |                                  |
| UART3 TX | B10 | 2,3(!)| ?,? | serial_tx 2 |                                 |
| UART3 RX | B11 | 2,4(!)| ?,? | serial_rx 2                                 |
| CRNT     | C1  | ---   | 2,4 | adc_curr                                |
| VBAT     | C2  | ---   | 2,4 | adc_batt                                 |
| UART6 TX | C6  | ?,?   | ?,? | serial_tx 3                                 |
| UART6 RX | C7  | ?,?   | ?,? | serial_rx 3                                 |
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