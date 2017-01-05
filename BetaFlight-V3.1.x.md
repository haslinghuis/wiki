##Betaflight 3.1.0-RC1  

Betaflight 3.1.0 (Release Candidate 1)  

Betaflight firmware has undergone some major changes under the hood. Hardware drivers have been optimized to improve future maintainability, but also easier target and hardware support. The efficiency of the code has also been improved by a lot as Betaflight team reviewed each line of the code to squeeze every possible performance win out of it for flight performance purposes. The difference between the current release and previous one is over 1600 code commits by various developers. Only release notes highlights are represented. For full change history github commit history can be reviewed.  

Release notes highlights:

- Added F7 support (ANYFCF7 as the first supported target...)
- Dynamic IO / pin allocation
- [DSHOT Support](/betaflight/betaflight/wiki/DSHOT ESC Protocol) for F3 and F4 (read wiki about board supported hardware)
- Full Floating Point Logic for flight behavior
- Many new dynamic configurations (filters, setpoint weights etc.)
- Support for KISS esc telemetry (only with DSHOT)
- Many code optimizations (higher looptimes possible)
- Added [Serial ESC Pass-through](/betaflight/betaflight/wiki/Betaflight-ESC-pass-through) for KISS24 and CASTLE esc's
- New target support (now 65 targets)
- Added CMS display support
- Added CRSF support for TBS receivers and associated telemetry
- Added additional OSD parameters like pids and power
- Added [Unify SmartAudio](/betaflight/betaflight/wiki/Unify-Smartaudio) support
- Auto Video Format support for OSD
- New "anti_gravity_threshold" parameter to improve stability in fast changing G forces during flight. This applies to quick throttle jumps where multirotor can go through weightless transitions. In these cases the iterm can cause unwanted effects like pitching up or yawing due to strong changes in accumulation polarities.
- Protection against too fast motor speeds (When ONESHOT125 selected for example, max allowed pid and motor speed will be 2khz) and many more.. 

NOTE- It is recommended to use:  
- Configurator 1.8.5 or higher for this release
- BlackBox Viewer 2.5.8

###Bugs and fixes:  
- RC Expo messes with roll rates. 
RC2 will be available very soon. It was a small polarity miscalculation caused by last cleanups, Boris.  
 
###New CLI commands for 3.1:

####Resource Remapping
From betaflight v3.1 there is a new command to map resources. No more custom motor mixes just to move a motor pin.  
[Resource Mapping](/betaflight/betaflight/wiki/Betaflight-resource-remapping) goes into further details on how to use this new command.  

####set digital_idle_percent = ?
<i>[0..??]<i>  
See [Setting Min Throttle with Dshot](/betaflight/betaflight/wiki/DSHOT ESC Protocol)

####set anti_gravity_threshold = ?  
<i>[0..??]<i>  
See Release Notes above for brief description.  
