##Betaflight 3.1.0-Release  

Link to Releases:  
https://github.com/betaflight/betaflight/releases   
Note: Embedded links to Videos are in the Github Release Notes.   

Read here to Learn exactly what firmware issues and features are being worked on:
https://github.com/betaflight/betaflight/issues?q=is%3Aissue+is%3Aclosed+sort%3Aupdated-desc  

###Betaflight 3.1.0 (Release)   

Betaflight firmware has undergone some major changes under the hood. Hardware drivers have been optimized to improve future maintainability, but also easier target and hardware support. The efficiency of the code has also been improved by a lot as Betaflight team reviewed each line of the code to squeeze every possible performance win out of it for flight performance purposes. The difference between the current release and previous one is over 1800 code commits by various developers. Only release notes highlights are represented. For full change history github commit history can be reviewed.  

####Release note highlights:

- Added F7 support with already few supported targets - @sambas
- Dynamic IO / pin allocation - @blckmn
- [DSHOT Support](/betaflight/betaflight/wiki/DSHOT ESC Protocol) for F3 and F4. DSHOT150, 300, 600, and 1200 supported (read wiki about board supported hardware) - @blckmn
- Full Floating Point Logic for flight behavior - @borisbstyle
- Many new dynamic configurations (filters, setpoint weights etc.) - @borisbstyle
- Many code optimizations (faster pid speeds possible on F3 and F4) - @martinbudden and @borisbstyle
- Support for KISS ESC telemetry (only with DSHOT) - @basdelfos
- Added temperature and RPM to KISS ESC telemetry - @mikeller 
- Added [Serial ESC Pass-through](/betaflight/betaflight/wiki/Betaflight-ESC-pass-through) for KISS24 and CASTLE esc's - @sambas
- New target support (now 72 targets on 4 MCU types)
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
- Added new level sensitivity and level limit parameters in degrees. level_limit is the maximum allowed angle. Level_sensitivity is the max deflection on full stick @borisbstyle  
- Added IRC Tramp VTX support. Changable channel, band, power and pitmode @jflyper
- and many more: https://github.com/betaflight/betaflight/commits/master  

NOTE- For the features in this release you will need to use the following Versions or Higher:  
- Configurator 1.9.0
- BlackBox Viewer 2.5.9

###Bugs and fixes:  
- RC2 - Fix in rc expo symmetry // fix missing baro on some targets
- RC3 - Enable experimental 32khz support
- RC4 - Fix non MPU INT supporting targets // Added MPU Int NAZE // Fix adjustment for setpoint // Some cleanups
- RC5 - Fix more non MPU INT supporting targets // fixed RACEBASE and some SPRACINGF3 variants // Fix ledstrip on BETAFLIGHTF3 and IMPULSERCF3 // DSHOT900 and DSHOT1200 added for testing (only to be enable through cli for now)  
- RC6 - Fix ledstrip IMPULSERCF3 // Fix DSHOT for SIRINFPV // Add PODIUMF4 // Improved CPU usage // Optimised RC interpolation // Improve DSHOT speed // Add more safety in DSHOT limits (DSHOT150 is limited to 4khz)  
- RC7 - Fix gyro detection handling for 32k mode // Improved target limitiation  
- RC8 - Fix FPV angle mix // Added RG_SSD_F3 target // SPRACINGF3NEO DSHOT optimalisations // CC3D_OPBL fix // Remove MSP from UART1 by default // Added debug for gyro calibration noise // Minor optimalisations    
- RC9 (Build #959 - 16Jan2017)- Fix servo mixer scaling for tricopters // Add softserial for NAZE // Add IRC Tramp VTX support // Fix FPV angle mix  
- RC10 (Build #965 - 19Jan2017)- Added anti_gravity_gain // KISSFC dshot support motor 5 and 6 // CC3D startup issue solved // new defaults for level and PID's  
- RC11 - Fix spectrum bind PIN on BFF3 // Fix connection to some targets // Restore missing blackbox log fields  
- RC12 ( -25Jan2017)- FPV angle mix applied to actual rates (also a fix) // Fix truncated blackbox logs // Redefined OSD defaults to not have PIDs by default on screen // Increased configurable filter range  
- RC13 - reported bug where blackbox would disable itself is now resolved   

#####Notes from Boris:   
DSHOT1200 does work now but only on kiss24 that I know.
We decided to add a lot of new stuff available from cli for testing purposes and try to only add proven things in the configurator.   

#####Questions and Answers about 3.1 from Boris' BetaFlight thread.    
######question by Woody_99:  
I've been flying a Naze32 on BF (3.01) for a while, and seems to be working fine for me.
With all the code optimizations (noted in the WIKI), is the Naze still a viable option to continue with, or should I swap it out for a newer FC?  
Answer from Boris:  
No NAZE32 and other F1s actually got slower in 3.1. 3.1 is the first version where everything is floating point math. F1 lacks of floating point processor unit so it gets a lot of more to work.
Besides that it only has 128k flash what prevents a lot of new optimizing we applied.
All optimizations only affect f3, f4 and f7 boards.  
Instead of running new versions of betaflight or reforking it you can simply stick to the version you use now for example. There is no solution for F1 boards in the future unfortunately and every new feature will NOT be included to it. You are for example willing to give up acc, but 100 others may not. So that's not a solution.  
I see that Softserial does fit again since latest cleanups (RC9?). There is only like 1kb left on Naze now.  

######question by fftunes:  
If i run 8k/1k, will the PID loop be calculated from an average of the 8 gyro samples, or will it only use 1 sample out of 8?  
Answer from Boris:  
There is no averaging. There is IIR filtering, what works faster than averaging. Every sample it's information is taken to the next sample a bit. Btw you can enable simple averaging with chosing FIR filter style. Averaging gives a lot of delay typically.  
It seems that a lot of guys really missed the early betaflights where all this was discussed a lot. All i can say is to read about the way how filtering works and look up about aliasing. (Filtering is explained in this Wiki)  

######Question by spikerspike97:  
But why do my throttle and yaw rccommands have so much steps and the pitch and roll are super smooth as seen in the BB log?   
Answer from Boris:   
Because only roll and pitch have Derivative kick affect. Therefore only those are smoothed by default. You can enable the full smoothing in cli, but I suggest fly it like this.  
Only reason for smoothed rc inputs is Derivative kick symptom where PIDsum can get very jerky.  

######To check for DMA conflicts do the following (thanks teralift):  
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

Note: See the [3.0.x page](/betaflight/betaflight/wiki/BetaFlight-3.0.x) for CLI commands plus other features that were new in 3.0.x  
See the [V2.x CLI Commands](/betaflight/betaflight/wiki/Betaflight-specific-CLI-commands) page for a history of CLI command changes. This WIKI has only documented Changes from being Forked from CleanFlight. Do see the CF docs.  

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
32khz is added just because it can, but no new harder filtering will be defaulted to that. If you want to fly 32khz you will have to try to optimise your filters by yourself.  
Default filtering is good enough for 8k gyro sampling, but 32khz requires more filtering depending of setup.
I did find out that old blheli esc's for example perform well under 32khz as those are less responsive, while blheli_s and other responsive esc's with better braking really suffer from micros on 32khz.  

###New RC6 CLI commands:  

Level mode has changed in 3.1 a bit. It got more parameters.  
In the Configurator pid tab you can find 2 new level parameters.  
It is level sensitivity and level limit.  
Both are in degrees.
Level sensitivity is the max angle on full stick and level limit is the limited angle.  
So for example sensitivity of 100 means that full stick would give you tilt of 100 degrees, but if the limit is just 70 degrees last 30% of your stick will be thrown away.  
Lowering your sensitivity will give you smoother stick control. Maybe the defaults are a bit aggressive perhaps.  
Rc rate or any other rate or expo parameter doesn't do anything for level modes.   

####set level_limit = 70
<i>[10..120]<i>  
 the maximum allowed angle in degrees  

####set Level_sensitivity = 100
<i>[10..200]<i>  
the max deflection on full stick in degrees  

###New RC10 CLI commands:  

####anti_gravity_gain = 4.000
<i>[1..30]<i>
Gain is the temporary iterm acceleration on rapid throttle moves.  
Boris: Well fly and see how it goes on defaults and post some logs if you can.   
