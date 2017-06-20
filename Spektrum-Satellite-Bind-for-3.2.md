### General Description

Prior to 3.2, Spektrum satellite bind pin was fixed to a pre-defined pin, usually a RX pin of fixed UART port.

With 3.2 and later,

- The bind pin automatically defaults to RX pin of the UART port the satellite receiver is configured; if you configure UARTx for serial RX and set serial RX protocol to one of SPEKTRUM kind, then UARTx_RX will be used for binding.

- If SRXL is selected as a serial RX protocol, then UARTx_TX will be used for binding.

### Overriding Bind Pin Default

The automatic bind pin selection can be overridden by specifying a pin to use by `RX_BIND` resource and `resource` cli command.
```
resource RX_BIND pin-id
```

### Bind Pin Override example - ASGARD

ASGARD's default serial RX pad is UART6_RX. However, since there is a uni-directional inverter on this pad, it can not be used as a bind pin. Fortunately, there is another MCU pin that is connected directly to this pad, which is PB8. The PB8 is used as PPM input when PPM is selected as a receiver input, but you can utilize this pin to drive receiver signal line.
To do this, override the default bind pin
```
resource RX_BIND B8
```
Then follow normal satellite binding procedures.