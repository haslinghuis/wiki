### Preliminary Notes on NOX

#### Inverter for inverted serial RX protocols
UART2_RX has an inverter, controlled by PC14, with pull-up.
BF3.2 has ability to control this inverter, by explicitly assigning PC14 as the control pin.
```
resource inverter 2 PC14
```

With BF3.1.7 and earlier, the inverter is always activated by the pull-up. This means that non-inverting serial RX protocols (such as IBUS and DBUS) can not be used for NOX with BF3.1.7 and earlier. Use BF3.2 for non-inverting serial RX protocols with above inverter resource mapping.

UART2_TX also has an inverter, also controlled by PC14, but without a pull-up.