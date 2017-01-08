<h3>Betaflight BLHeli ESC pass-through<h3>

1- Connect FC to computer and note COMM port number.  
2- Open BLHeli Suite. Select Comm Port and click Connect.  
3- In 'Select ATEM/SILBAS Interface' menu select 'SILABS BLHeli Bootloader (ClaenFlight)'.  
4- Apply power the ESCs (through a Current Limiter).  
5- Click 'Read Setup'.  

Now ready to change ESC settings or Flash new firmware. 

<h3>Betaflight KISS ESC pass-through<h3>

New feature for 3.1 allows to flash kiss escs with a pass-through and KISS flash loader app.  
Note: it is apparently not yet active on all targets. 

For now you have to enable passthrough in the cli and than flash through flash loader app. Later it might be done from the app directly. [Flyduino downloads](http://kiss.flyduino.net/downloads/)  

KISSESC_flashloader also in [First Post of Dshot Thread.](https://www.rcgroups.com/forums/showthread.php?2756129-Dshot-testing-a-new-digital-parallel-ESC-throttle-signal  )

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


<h3>Current Limiter<h3>   

Always use a Current Limiter when ever a LiPo is connected on the Bench for Testing or ESC Flashing, Calibrating or any time the Configurator is open.  
Light Bulb Current Limiter Build thread:   
https://www.rcgroups.com/forums/showthread.php?2327875-DIY-SAVE-YOUR-ELECTRONICS!-BUILD-A-SmokeStopper%C2%99-!