# Serial Pass through

## Update FrSky XSR RX Firmware
**NOTE: This needs verification and testing.** BryceJ has successfully done this procedure, however I was unable to replicate due to issues with the FrSky tool not loading COM ports.  If you are familiar with this procedure, please update this information.

### FrSky Tool
[https://www.frsky-rc.com/stk/](https://www.frsky-rc.com/stk/)
* Expand **Tool-FrSky update sport**
* Click the **Download** button
* Extract the contents (it's a .zip) and remember the location

### FrSky Rx Firmware
[https://www.frsky-rc.com/download/](https://www.frsky-rc.com/download/)

Download the firmware for your Rx (if you don't see your Rx, below, go to the download page (above) and find the correct firmware for your model Rx):
- [https://www.frsky-rc.com/xsr/](https://www.frsky-rc.com/xsr/)
- [https://www.frsky-rc.com/r-xsr/](https://www.frsky-rc.com/r-xsr/)
- [https://www.frsky-rc.com/xm-plus-mini-sbus-non-telemetry-full-range/](https://www.frsky-rc.com/xm-plus-mini-sbus-non-telemetry-full-range/)
- [https://www.frsky-rc.com/x4rsb/](https://www.frsky-rc.com/x4rsb/)
- [https://www.frsky-rc.com/r9-mini/](https://www.frsky-rc.com/r9-mini/)
- https://www.frsky-rc.com/r9-slim-plus/

### The Process
NOTE: This might not work in every case. To increase success rate, it's important to power the Rx from a 5 volt source other than a dedicated Rx power pad, _as you do not want the Rx to be powered by USB_.

* Open Betaflight Configurator, connect you copter, access CLI
* Enter and send command serialpassthrough 57600
* Disconnect power to the Rx (this is going to be the tricky part)
* Start the FrSky tool (where is this? What is it called?)
* Apply power to the Rx
* Update firmware via FrSky Tool

From BryceJ: 
On my Spracingf3 board with a CP210x it would only work if I had my MSP changed to 57600 as well. Not sure what is the issue there. (If you change the MSP speed make sure you reconnect to the configurator at 57600 as well)  

Here is the frsky windows tool: Link needs fixing  
https://www.frsky-rc.com/wp-content/...port_rev14.zip  
frsky_update_sport_rev14.exe  

If you use the PID/VTx adjustment on the taranis, make sure that still works. There was a couple versions that it didn't.  

For disconnecting the XSR, if you have the plug still on there you can plug and unplug that. On a x4r it would be a little more annoying if you direct soldered it.  


## minimOSD Serial Pass-through

Allows use of the MW OSD Configurator to adjust settings and load fonts.

[Video](https://www.youtube.com/watch?v=5ABd0gz3ckI)