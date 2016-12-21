<h3>Betaflight ESC pass-through<h3>

New feature for 3.1 allows to flash kiss escs with a pass-through and KISS flash loader app.  
Note: it is apparently not yet active on all targets. 

For now you have to enable passthrough in the cli and than flash through flash loader app. Later it might be done from the app directly. [Flyduino downloads](http://kiss.flyduino.net/downloads/)  

KISSESC_flashloader also in First post of the Dshot thread:  
https://www.rcgroups.com/forums/showthread.php?2756129-Dshot-testing-a-new-digital-parallel-ESC-throttle-signal  

Command for esc #1:
escprog ki 1

Command for esc #2:
escprog ki 2

Command for all 4 escs at the same time:
escprog ki 255


Procedure is easy:
- Enter above commands in the cli and disconnect from configurator by clicking disconnect button.
- Go to kiss flash loader app and select com port to connect.
- Select USB-uart
- Select dshot hex file to flash
- You can enable fast bootloader for faster flashing.
- Press flash and you will see led blinking on the escs.
- Disconnect lipo and usb cable and you are now able to use dshot

If the above command doesnt work escserial flash passthrough is not enabled yet.  
Btw also castle escs can be flashed on the same way.  