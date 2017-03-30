# OMNIBUS F4 AIO, F4 V2/V3 and F4 Pro

### Note for OMNIBUS F4 V3 and ~~F4 Pro V3~~ users

OMNIBUS F4 V3 ~~and F4 Pro V3~~ SBUS header through-hole is connected to **UART6**, not UART1 like all others.

For board identification, see
[OMNIBUS F4 V2 & V3 : Identifying revisions](https://www.rcgroups.com/forums/showthread.php?2837385-OMNIBUS-F4-V2-Identifying-revisions)

### LED strip pin assignment will change with v3.2

v3.1.x and earlier uses PWM5 (OMNIBUSF4 target) or PWM6 (OMNIBUSF4SD target) for LED strip, not the LED labelled pin. This was a work around for the original OMNIBUS F4 AIO having invalid pin on the designated pin. However, newer revisions of the OMNIBUS F4 family has valid (usable) pin assigned to the LED pin. Unfortunately, the pin is not ready for use with BF3.1.x, but will be in v3.2 as a default. It means the current wiring will not work unless explicitly remapped to the current pin.
Please be advised to take a look at this page when v3.2 is released.

#### LED strip pin assignment is already changed in the master

The LED strip pin assignment is already changed in the master as of Mar.29, 2017.

Those users with working LED strip on PWM6 can either migrate to the new assignment if the board supports the mapping, or explicitly assign older mapping with the `resource` CLI command.

1. Migrating to the new assignment

Users of the following boards can migrate (reconnect the LED strip signal wire) to the new assignment that uses the designated LED through-hole/connector.

- OMNIBUS F4 V2 (J9)
- OMNIBUS F4 V3 (J9)
- OMNIBUS F4 Pro V3 (J1)

Notes:
(1) Firmware is already modified to use these through-hole/connector.
(2) Unfortunately, PPM users can't use this new assignment because The new pin (MCU PB6) has a timer collision with PPM input pin (PB8); these users are forced to use the remap method below.

2. Explicit pin assignment remapping.

Users who choose (or forced in PPM case) not to migrate to the new assignment have to explicitly remap the assignment to the older default using the resource CLI command below.

```
resource led_strip a8
```

### Board naming (needs updating)
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
| PWM5     | A1  | 5,2   | 1,4 | motor 5, led_strip                         |
| PWM4     | A2  | 2,3   | 1,1 | motor 4 |                                  |
| PWM3     | A3  | 2,4   | 1,6 | motor 3 |                                  |
| PWM6     | A8  | 1,1   | 2,3 | motor 6 |
| UART1 TX | A9  | 1,2   | 2,2 | serial_tx 1 | Not remappable with v3.1     |
| UART1 RX | A10 | 1,3   | ?,? | serial_rx 1 | Not remappable with v3.1     |
| PWM1     | B0  | 3,3   | 1,7 | motor 1 |                                  |
| PWM2     | B1  | 3,4   | 1,2 | motor 2 |                                  |
| LED      | B6  | 4,1   | 1,0 |         | J9 (Not accessible with v3.1)    |
| PPM      | B14 | 12,3  | n.a | ppm     |                                  |
| CH2      | B15 | 12,4  | n.a |         |
| UART3 TX | B10 | 2,3(!)| ?,? | serial_tx 2 |                              |
| UART3 RX | B11 | 2,4(!)| ?,? | serial_rx 2 |                              |
| CRNT     | C1  | ---   | 2,4 | adc_curr|                                  |
| VBAT     | C2  | ---   | 2,4 | adc_batt|                                  |
| UART6 TX | C6  | 8,1   | 2,2 | serial_tx 3 |
| UART6 RX | C7  | 8,2   | 2,3 | serial_rx 3 |
| CH5      | C8  | 8,3   | 2,4 |         |
| CH6      | C9  | 8,4   | 2,7 |         |

### OMNIBUS F4 V2 and F4 Pro

- Use OMNIBUSF4SD target.
- LED strip port is PWM6.

#### Resource Mapping (WIP)

(Not available yet)

## Other Resources

RCgroups thread for the F4 AIO at <https://www.rcgroups.com/forums/showthread.php?2754926-Omnibus-F4-AIO>

RCgroups thread for the F4 Pro at <https://www.rcgroups.com/forums/showthread.php?2801694-Omnibus-f4-pro>

Layouts for F4 Pro [Top](https://www.rcgroups.com/forums/showatt.php?attachmentid=9631520&d=1482680395) [Bottom](https://www.rcgroups.com/forums/showatt.php?attachmentid=9631521&d=1482680397)

## Receiver Setup

### Serial RX

On the OMNIBUSF4, the UART1 RX pin is available for use on 3 different headers, only one of which can be used at any given time:
  - SBUS port (via inverter), this pin is also shared with the PPM pin.
  - Spektrum sat header (no inverter)
  - UART1 header (no inverter)

### Spektrum Binding (v3.1.6+)

- Configure the flight controller for your receiver by opening the BetaFlight Configurator and on the ports page, set UART1 to Serial RX and click save. Switch to the configuration tab and in the Receiver section, set the mode to SerialRX and provider to either:

  - Spektrum 2048 for DSMX
  - Spektrum 1024 for DSM2

- In the CLI run:

```
set spektrum_sat_bind = 9
set spektrum_sat_bind_autorst = 0
save
```

- Wait for the board to reboot, then remove all power from the board (unplug the USB), wait a moment then and plug the in the USB cable.

- The bind light on the receiver should be flashing.

- Turn on your transmitter in bind mode.

- The flashing light on the receiver should now be solid.

- Turn of your transmitter.

- Finally take the receiver out of bind mode by running the following in the CLI:

```
set spektrum_sat_bind = 0
save
```

- Make sure to change to TAER channel order under the Receiver tab in the BetaFlight configurator.