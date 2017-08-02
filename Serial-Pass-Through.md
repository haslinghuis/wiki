# Serial Pass through

## Update FrSky XSR RX Firmware

This is what it seems to me (using a fully build copter with Rx wired and powered by USB)
Open Betaflight Configurator, access CLI
Enter and send command serialpassthrough 57600
Disconnect power to the Rx (this is going to be the tricky part)
Start the FrSky tool (where is this? What is it called?)
Apply power to the Rx
Update firmware via FrSky Tool

On my Spracingf3 board with a CP210x it would only work if I had my MSP changed to 57600 as well. Not sure what is the issue there. (If you change the MSP speed make sure you reconnect to the configurator at 57600 as well)

Here is the frsky windows tool:
https://www.frsky-rc.com/wp-content/...port_rev14.zip
frsky_update_sport_rev14.exe

If you use the PID/VTx adjustment on the taranis, make sure that still works. There was a couple versions that it didn't.

For disconnecting the XSR, if you have the plug still on there you can plug and unplug that. On a x4r it would be a little more annoying if you direct soldered it.
