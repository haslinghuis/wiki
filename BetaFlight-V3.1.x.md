##Betaflight 3.1.0-RC3  

Betaflight 3.1.0 (Release Candidate 3)  

Betaflight firmware has undergone some major changes under the hood. Hardware drivers have been optimized to improve future maintainability, but also easier target and hardware support. The efficiency of the code has also been improved by a lot as Betaflight team reviewed each line of the code to squeeze every possible performance win out of it for flight performance purposes. The difference between the current release and previous one is over 1600 code commits by various developers. Only release notes highlights are represented. For full change history github commit history can be reviewed.  

Release notes highlights:

- Added F7 support with already few supported targets
- Dynamic IO / pin allocation
- [DSHOT Support](/betaflight/betaflight/wiki/DSHOT ESC Protocol) for F3 and F4 (read wiki about board supported hardware)
- Full Floating Point Logic for flight behavior
- Many new dynamic configurations (filters, setpoint weights etc.)
- Support for KISS esc telemetry (only with DSHOT)
- Many code optimizations (higher looptimes possible on F3 and F4)
- Added [Serial ESC Pass-through](/betaflight/betaflight/wiki/Betaflight-ESC-pass-through) for KISS24 and CASTLE esc's
- New target support (now 66 targets)
- Added CMS display support
- Added CRSF support for TBS receivers and associated telemetry
- Added additional OSD parameters like pids and power
- Added [Unify SmartAudio](/betaflight/betaflight/wiki/Unify-Smartaudio) support
- Auto Video Format support for OSD
- New "anti_gravity_threshold" parameter in CLI
- Protection against too fast motor speeds (When ONESHOT125 selected for example, max allowed pid and motor speed will be 2khz) and many more.. 
- Added experimental 32khz support for gyros that support it. Usually F4 board will run fine on 32khz gyro and 16khz pid loop. 32/32 is slightly too much for CPU. F7 target is now the only one able to run 32khz/32khz flawlessly with even accelerometer enabled. and many more. To enable 32khz mode use cli setting "gyro_use_32khz = ON". (Configurator will not display correct speed until the next configurator update, but you will see the real cycletime)  

NOTE- You will need to use:  
- Configurator 1.8.5 or higher for this release
- BlackBox Viewer 2.5.8

###Bugs and fixes:  
- RC2 - Fix in rc expo symmetry // fix missing baro on some targets
- RC3 - Enable experimental 32khz support

###New CLI commands for 3.1:

####Resource Remapping
From betaflight v3.1 there is a new command to map resources. No more custom motor mixes just to move a motor pin.  
[Resource Mapping](/betaflight/betaflight/wiki/Betaflight-resource-remapping) goes into further details on how to use this new command.  

####set digital_idle_percent = 3.000
<i>[0..20]<i>  
Only used when a DSHOT ESC protocol is selected.  
See [Setting Min Throttle with Dshot](/betaflight/betaflight/wiki/DSHOT ESC Protocol)

####set anti_gravity_threshold = 350  
<i>[20..1000]<i>  
 To improve stability in fast changing G forces during flight. This applies to quick throttle jumps where multirotor can go through weightless transitions. In these cases the iterm can cause unwanted effects like pitching up or yawing due to strong changes in accumulation polarities.  

####set yaw_accel_limit =  20.000
<i>[0..50]<i>  
Note from Boris: The old value was upscaled. This is the real value now in float representation.  
Its representing deg/sec/ms. A bit easier to swallow for human.  

###New RC3 CLI commands:

####set gyro_use_32khz = OFF  
<i>[OFF..ON]<i>  
Only available on F4 & F7 targets.
