# Howto setup Spektrum SPM4649T SRXL Telemetry

It's easy:

1. Connect your SPM4649T to the TX3 pin on your FC (and feed it 5V obviously).
2. In Betaflight Configurator Ports page enable 'Serial RX' on UART3 and save
3. After board has rebooted, in the Configuration page select Serial Receiver and SRXL as provider.
4. Turn on the TELEMETRY feature. Press "Save and Reboot".

Please also note that it does not have to be connected to UART3/TX3 as stated above, any free UART Tx pin will do fine, PROVIDED there is a direct connection to the processor IO-pin. I.e. no inverters or such in the way, blocking bidirectional data flow. UART3 is usually fine on most FCs. UART1 and 6 might be more troublesome on F4 based FCs, depending on FC brand and design. 

## The information you can get via Spektrum Telemetry.

* RSSI, Frame drops and Holds.   
* Battery Voltage. Min/Max and current value.
* Battery Current and Capacity used.
* Betaflight Configuration Menu System, CMS. 
* VTX Status (In a separate VTX  Setup menu)

Use the transmitter scroller to select the items you like to use.

##Telemetry sample screen shots.

![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Overview.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Flightlog.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Voltage_MinMax.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Voltage.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_FlightPackCapacity.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_TM_Text_CMS.jpg)
![Spektrum Telemetry Overview](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/Spektrum_VTX_Status.jpg)
