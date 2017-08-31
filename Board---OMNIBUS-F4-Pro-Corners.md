## OMNIBUS F4 Pro Corners

## Hardware

- Variant of OMNIBUS F4 Pro (OMNIBUSF4SD target). Please refer to OMNIBUS F4 wiki page for additional information.
- Still uses OMNIBUSF4SD target.
- Two dual inverters, on UART3 (controlled by PC9) and UART6 (controlled by PC8); supports SBUS and native SmartPort (requires a diode for SmartPort).
- UART1_RX can be connected to ESC telemetry via jumper.

## Configuration Example

- UART1 ESC Telemetry
- UART3 SBUS
- UART6 SmartPort (Inverted, TX-Inline-Diode)
- SOFTSERIAL1 SmartAudio

![](https://user-images.githubusercontent.com/14850998/29904533-3ec5c1f6-8e44-11e7-879f-e1b433b4d8f1.jpg)

```
# Betaflight / OMNIBUSF4SD (OBSD) 3.2.0 Aug 28 2017 / 12:02:37 (b2cd7294e)

# resources
resource SERIAL_TX 1 NONE
resource SERIAL_TX 11 A09
resource INVERTER 3 C09

# feature
feature SOFTSERIAL
feature TELEMETRY
feature ESC_SENSOR

# serial
serial 0 1024 115200 57600 0 115200
serial 2 64 115200 57600 0 115200
serial 5 32 115200 57600 0 115200
serial 30 2048 115200 57600 0 115200

# master
set serialrx_provider = SBUS
set current_meter = ESC
set battery_meter = ADC
set tlm_halfduplex = OFF
```