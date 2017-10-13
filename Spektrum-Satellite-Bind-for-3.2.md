### General Description

Prior to 3.2, Spektrum satellite bind pin was fixed to a pre-defined pin, usually a RX pin of fixed UART port.

With 3.2 and later, you have a freedom of moving the Spektrum satellite receiver around with bind facility following your choice of UART.

- The bind pin automatically defaults to RX pin of the UART port the satellite receiver is configured; if you configure UARTx for serial RX and set serial RX protocol to one of SPEKTRUM kind, then UARTx_RX will be used for binding.

- If SRXL is selected as a serial RX protocol, then UARTx_TX will be used for binding.

### Overriding Bind Pin Default

The automatic bind pin selection can be overridden by specifying a pin to use by `RX_BIND` resource and `resource` cli command.
```
resource RX_BIND pin-id
```

There are targets with `RX_BIND` by default, which disables the automatic port selection. In this case, you can remove this assignment by the following command.
```
resource RX_BIND NONE
```

### Bind Plug

The bind plug facility is also reconfigurable with 3.2.

Use resource`RX_BIND_PLUG` to specify the bind plug pin.
```
resource RX_BIND_PLUG pin-id
```

Note that only the `Negative logic` plug (short to ground to make it work) type is supported (for now).


### Binding Sequence once all is setup. This is the Same as has been.
In the CLI:   
`set spektrum_sat_bind = 9  `   
`save  `   

Unplug USB cable, plug it back in, when the LED flashes bind it to the controller.   

`set spektrum_sat_bind = 0   `   
`save  `   

other bind options  
3 for dsm2 1024/22ms  
5 for dsm2 2048/11ms  
7 for dsmx 1024/22ms  
8 for dsmx 2048/22ms  
9 for dsmx 2048/11ms  

Note: Binding will only work if there are a direct connection between the FC processor and the satellite, allowing for bi-directional data. On REVO, and all other F4 based FCs there are inverters on some UARTs that will block the binding signal. UART3 is usually OK on most F4s, UART1 and 6 might be more troublesome.  