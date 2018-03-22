# Howto setup Spektrum SPM4649T SRXL Telemetry

It's easy:

1. Connect your SPM4649T to +5V, GND and a free UART TX pin on your FC. For example UART3/TX3.  On a Piko F4 OSD board, the pad you need is UART4/TX4. Do NOT connect the VBAT/GND input on this Rx, this input is only used if there are no telemetry from the FC.
2. In Betaflight Configurator Ports page, enable 'Serial RX' on the UART you connected to above, and save.
3. After the board has rebooted, Go to the Configuration page.  In the Receiver box you select "Serial based Receiver" as Receiver Mode and Serial Receiver Provider: "SPEKTRUM2048/SRXL" (or "Spektrum Bidir SRXL" in Configurator 10.2.0 and older).
4. Turn on the TELEMETRY feature. Press "Save and Reboot".
5. Bind the SPM4649T to your RC Transmitter, for details see https://github.com/betaflight/betaflight/wiki/Spektrum-Satellite-Bind-for-3.2
6. Select Channel map Spektrum/Graupner/JR (TAER1234) in the receiver tab.  


Please also note that it does not have to be connected to UART3/TX3 as stated above, any free UART Tx pin will do fine, PROVIDED there is a direct connection to the processor IO-pin. I.e. no inverters or such in the way, blocking bidirectional data flow. UART3 is usually fine on most FCs.  UART1 and 6 might be more troublesome on F4 based FCs, depending on FC brand and design. 
 
You must also do a final step on your RC Transmitter.

7. Run "Auto-Config" in the "Function List" "Telemetry" menu.

If your SPM4649T are connected to TX3, you can also open CLI and copy & Paste this:

`serial 2 64 115200 57600 0 115200`    
`feature RX_SERIAL`    
`feature TELEMETRY`    
`set serialrx_provider = SRXL`    
`map TAER1234`    
`save`    


## The information you can get via Spektrum Telemetry.

* RSSI, Frame drops and Holds.   
* Battery Voltage. Min/Max and current value.
* Battery Current and Capacity used.
* Betaflight Configuration Menu System, CMS. 
* VTX Status (In a separate VTX  Setup menu)

Use the transmitter scroller to select the items you like to use.

Note: When using CMS in this way, it will be given highest possible priority on the telemetry radio link for performance reasons. A few other telemetry reports will be disabled as long as you are using CMS. This makes it VERY IMPORTANT to leave CMS with a proper EXIT, otherwise you will loose some telemetry data. Like battery current and capacity used, VTX status etc. 

## Telemetry sample screen shots.

![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Overview.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Flightlog.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Voltage_MinMax.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Voltage.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_FlightPackCapacity.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Text_CMS.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_VTX_Status.jpg)
