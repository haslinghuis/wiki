##Betaflight 3.1.0-RC7  

Link to Releases:  
https://github.com/betaflight/betaflight/releases   

Betaflight 3.1.0 (Release Candidate 7)  

Betaflight firmware has undergone some major changes under the hood. Hardware drivers have been optimized to improve future maintainability, but also easier target and hardware support. The efficiency of the code has also been improved by a lot as Betaflight team reviewed each line of the code to squeeze every possible performance win out of it for flight performance purposes. The difference between the current release and previous one is over 1600 code commits by various developers. Only release notes highlights are represented. For full change history github commit history can be reviewed.  

####Release note highlights:

- Added F7 support with already few supported targets - @sambas
- Dynamic IO / pin allocation - @blckmn
- [DSHOT Support](/betaflight/betaflight/wiki/DSHOT ESC Protocol) for F3 and F4. DSHOT150, 300, 600, and 1200 supported (read wiki about board supported hardware) - @blckmn
- Full Floating Point Logic for flight behavior - @borisbstyle
- Many new dynamic configurations (filters, setpoint weights etc.) - @borisbstyle
- Support for KISS esc telemetry (only with DSHOT) - @basdelfos
- Many code optimizations (faster pid speeds possible on F3 and F4) - @martinbudden and @borisbstyle
- Support for KISS ESC telemetry (only with DSHOT) - @basdelfos
- Added temperature and RPM to KISS ESC telemetry - @mikeller 
- Added [Serial ESC Pass-through](/betaflight/betaflight/wiki/Betaflight-ESC-pass-through) for KISS24 and CASTLE esc's - @sambas
- New target support (now 67 targets)
- Added CMS display support - @jflyper
- Added CRSF support for TBS receivers and associated telemetry - @martinbudden and @blckmn
- Added additional OSD parameters like pids and power - @martinbudden and @rafl
- Added [Unify SmartAudio](/betaflight/betaflight/wiki/Unify-Smartaudio) support - @jflyper
- Added MSP over Smartport - @raphaelcoeffic
- Auto Video Format support for OSD
- Configurator enhancements - @mikeller
- Speeded up build system, needed now there are so many targets - @AndersHoglund
- Fixed JUMBO frame handling on VCP targets, so blackbox logs can be downloaded more quickly - @AndersHoglund 
- New "anti_gravity_threshold" parameter in CLI - @borisbstyle
- Protection against too fast motor speeds (When ONESHOT125 selected for example, max allowed pid and motor speed will be 2khz) and many more..  - @borisbstyle
- Added experimental 32khz support for gyros that support it - @martinbudden. 
- Blackbox enhancements (use 2.5.8 blackbox-viewer) @GaryKeeble
- and many more.  
- Added new level sensitivity and level limit parameters in degrees. level_limit is the maximum allowed angle. Level_sensitivity is the max deflection on full stick @borisbstyle  

NOTE- For the features in this release you will need to use the following Versions or Higher:  
- Configurator 1.8.7
- BlackBox Viewer 2.5.8

###Bugs and fixes:  
- RC2 - Fix in rc expo symmetry // fix missing baro on some targets
- RC3 - Enable experimental 32khz support
- RC4 - Fix non MPU INT supporting targets // Added MPU Int NAZE // Fix adjustment for setpoint // Some cleanups
- RC5 - Fix more non MPU INT supporting targets // fixed RACEBASE and some SPRACINGF3 variants // Fix ledstrip on BETAFLIGHTF3 and IMPULSERCF3 // DSHOT900 and DSHOT1200 added for testing (only to be enable through cli for now)  
- RC6 - Fix ledstrip IMPULSERCF3 // Fix DSHOT for SIRINFPV // Add PODIUMF4 // Improved CPU usage // Optimised RC interpolation // Improve DSHOT speed // Add more safety in DSHOT limits (DSHOT150 is limited to 4khz)  
- RC7 - Fix gyro detection handling for 32k mode // Improved target limitiation  

#####Note from Boris: dshot1200 do work now but only on kiss24 that I know.
We decided to add a lot of new stuff available from cli for testing purposes and try to only add proven things in the configurator.   

To check for DMA conflicts do the following (thanks teralift):  
(1) Disable Dshot, enable LED_STRIP, save & reboot.  
(2) Goto CLI.  
(3) Type "resource list".  
(4) At the end of the list, there is DMA section. Record which DMA resource the LED_STRIP is using.  
(5) Type "exit".  
(6) Enable Dshot, disable LED_STRIP, save & reboot.  
(7) Goto CLI.  
(8) Type "resource list".  
(9) Check if any of DMA resource assigned to motors is same as the one LED_STRIP is using.   

###New CLI commands for 3.1:

Note: See the [3.0.x page](/betaflight/betaflight/wiki/BetaFlight-3.0.x) for CLI commands plus other features that were new in 3.0

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

####set gyro_isr_update = OFF  
<i>[OFF..ON]<i>  
From mjbudden:   
gyro_isr_update is an experimental feature I have added. When set on, the gyro is read and filtered in the ISR (interrupt service routine). This is "unconventional" programming practice (many would frown upon doing this), which is why the default is off.  
Theoretically setting it on should produce some small performance improvements, but that needs to be confirmed by flight testing. This setting should be used with caution.   
From Boris:   
Might be useful on slower i2c targets like NAZE etc. Its for testing purposes. Things not mentioned in release notes and manuals are not meant to be changed generally unless you really want to be a "tester".   

###New RC3 CLI commands:

####set gyro_use_32khz = OFF  
<i>[OFF..ON]<i>  
Only available on F4 & F7 targets.  
Usually F4 board will run fine on 32kHz gyro and 16kHz pid loop. 32/32 is slightly too much for CPU. F7 target is now the only one able to run 32kHz/32kHz flawlessly with even accelerometer enabled. To enable 32kHz mode use CLI setting gyro_use_32khz = ON. (Configurator will not display correct speed until the next configurator update, but you will see the real cycletime). NOTE - only flight controllers with MPU6500, MPU9250, and ICM-series (eg ICM20689) gyro support 32kHz mode.  

###New RC6 CLI commands:
####set level_limit = ??
 the maximum allowed angle  

####set Level_sensitivity = ??
the max deflection on full stick  
