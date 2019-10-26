Betaflight has the ability to display on your OSD a measurement of Crossfire LQ (Link Quality).  This measurement is based on a ratio of successful packets transmitted and received.  This is the recommended performance indicator to use instead of RSSI, due to the overall signal strength of the Crossfire hardware.

prior versions. 

To set it up, simply do the following:
1. Configure your Crossfire RX to transmit LQ on channel 12 by using the Crossfire OLED menu or Lua Script.
2. Alternatively, if the receiver is configured to use 8 channels only, configure LQ on channel 8
3. Configure your flight controller to use the CRSF protocol.
4. On the Receiver page, set the RSSI Channel to AUX 8.
5. Alternatively, if you setup LQ on channel 8, set the RSSI Channel to AUX 4.
6. Enable and place the RSSI element on your OSD.


Betaflight  4.1 has Native crossfire.
 

1. Configure your flight controller to use the CRSF protocol
2. On the Receiver page, set the RSSI Channel to disabled.
3. select lq in the osd menu.

### ** !!! IMPORTANT: DO NOT ENABLE THE RSSI_ADC SWITCH AS IT WILL LEAD TO INACCURATE RESULTS !!! **
