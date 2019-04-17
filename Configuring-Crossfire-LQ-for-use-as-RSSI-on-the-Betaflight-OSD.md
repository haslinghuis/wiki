Betaflight has the ability to display on your OSD a measurement of Crossfire LQ (Link Quality).  This measurement is based on a ratio of successful packets transmitted and received.  This is the recommended performance indicator to use instead of RSSI, due to the overall signal strength of the Crossfire hardware.

To set it up, simply do the following:
1. Configure your Crossfire RX to transmit LQ on channel 12 by using the Crossfire OLED menu or Lua Script.
2. Configure your flight controller to use the CRSF protocol.
3. On the Receiver page, set the RSSI Channel to AUX 8.
4. Enable and place the RSSI element on your OSD.

### ** !!! IMPORTANT: DO NOT ENABLE THE RSSI_ADC SWITCH AS IT WILL LEAD TO INACCURATE RESULTS !!! **