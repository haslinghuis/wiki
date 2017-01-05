This page is meant to provide a more in depth explanation of the changelog for each release so that users can understand the consequences of the changes made. Please remember to read the changelog for each release first and read the "Betaflight specific CLI commands" in this Wiki for new and changed CLI commands.

#Betaflight 3.0.0, 3.0.1 & 3.1
Please see the [Betaflight 3.x](/betaflight/betaflight/wiki/BetaFlight-3.x) page for release notes and details.

#2.9.0 (Configurator Integration)

- Fixed PID controller reset from Configurator
- Fixed Airmode Feature bug when Motor Stop enabled
- Added more Configurable options for the new Betaflight 1.3 Configurator
- Changed the default parameters (All accessible in Configurator)
- Default Derivative method is changed back to Measurement. To get the 2.8.1 behaviour you can change it back to Error in the new configurator. You can play with this setting to find your preferenced style. Freestyle pilot would prefer more Measurement method and racing pilots might like tigher stick feel with delta on Error. Delta from measurement will feel more smooth and natural, while delta from Error may feel a bit more robotic.  
D from error accelerates your stick input and gives you the ultimate sharp response on the stick.
D from measurements dampens your stick input and makes flip and roll stops much smoother.
- New default filters. These are now all accessible in the configurator
- Added new parameter (rc_smooth_interval_ms). Dont use it if you dont know what you are doing. 0 means disabled
- The PIDC names changed from LUX to Float and from MWREWRITE to Interger since they have been changed so much they are basically the same and not longer use the algorithms Lux & rewrite originally used.

Download the Betaflight Configurator for optimal Configuring experience
https://chrome.google.com/webstore/detail/betaflight-configurator/kdaghagfopacdngbohiknlhcocjccjao?hl=en

# Patch 2.8.1 RC2 -

Patch 2.8.1 Fixes:

- Fixed sensitivity in dynamic Iterm (prevent drift issues)
- Improved Dterm math replaces dynamic Pterm
- Some code cleanups
- Fixed issue where "dump all" command switches from rateprofile
- New defaults
- Increased Multishot resolution
- Improved Saturation Handling

Note: no need to do a full Erase if updating from 2.8.1 RC1. Settings will not be over written.

####Bugs:
- There was/is a problem with airmode not overriding motor_stop when airmode is used as a feature. Fixed now and will be released eventually.

# 2.8.1 RC1 - 
Is in Alpha testing and should fix a few Iterm issues.

Betaflight 2.8.1 (pre release)

Release 2.8 ("Rates Redesign")

- Changed Airmode Low Throttle Protection. Airmode now gets activated when stick moved above certain threshold. (CLI: set airmode_activate_throttle = ). Note that 3D users have to reconfigure their threshold to something else otherwise airmode would be always enabled.
- Sligthly more Authority on low throttle even without Airmode
- Added dynamic ki to dynamic PID feature. Iterm windups now won't increase instability. Iterm is better smoothed in dynamic situations
- Some code cleanups
- New default PIDs
- Added ledstrip visual beeper (CLI: ledstrip_visual_beeper)
- yaw_jump_prevention is removed and replaced by yaw D gain! The higher the D the lower the jump_prevention limit will be.
- Seperated fast_pwm_protocol from ONESHOT125 feature. The feature in GUI will only be green when ONESHOT125 is actually selected. Normal traditional PWM can be now selected with "set fast_pwm_protocol = OFF"
- Fix for beeper during boot. (Not beeping anymore)
- On the fly Rc Expo calculations (No more pre computed points)
- Experimental Anti ESC desync option. FC's can be pretty rough when it comes to throttle changes towards the ESC. This option allows you to set the maximum power increment / decrement to the ESC. You can declare the maximum change within 1ms. (CLI: set anti_desync_power_step = )
- Reworked Super Expo is now by default on as feature SUPEREXPO_RATES. You will like the new smoother stick feel! Check Rate calculator for rates. You still can disable it to get the old style rates. The new rates have smoothed mid stick so I don't recommend using the deadband anymore. Also there are no more super expo factors
- Yaw rate is same scaling like roll and pitch. It only has the fixed Rc Rate of 1.00 (100)
- SUPEREXPO_RATES and AIRMODE are both features now! When features enabled that means that airmode or superexpo are always enabled. When feature AIRMODE disabled it still can put on a switch. (Use feature command to enable or disable airmode)
- Added new Target SINGULARITY (Built in VTX)
- Added Rc Rate for Yaw! Will in the future also be added in the configurator. ("set rc_rate_yaw = ")

- Added rate limitations to 2000deg/sec. You cannot ever set the rates higher than the gyro can handle.

- New rate calculations allows to change everything via Rates and RC Rate. You actually dont even need to use RC Expo, but you still can. Check the rate calculator!

- Rate Calculator  
https://dl.dropboxusercontent.com/u/31537757/betaflight/Betaflight%20Rates%202.8.xls

NOTE  
2.8.0 VERSION HAS BEEN DEPRICATED DUE TO PROBLEMATIC ITERM TUNING. ALL ISSUES ARE SOLVED IN 2.8.1
YOU CAN DOWNLOAD 2.8.1 PRE RELEASE BELOW ALREADY BEFORE IT IS FULLY RELEASES

Joshua Bardwell's Betaflight 2.8.0 Release Overview video
https://www.youtube.com/watch?v=FrGaxDGx0fA

- Fixed Level Modes
- Prevent Iterm accumulating too much on full throttle
- Fix Blackbox Logging for Yaw
- When using anti desync and very high number like 10000 for example you get the old fashioned airmode_saturation_limit what also seriously can help some ESC's. (Like 2.1.6 version for example)

Note from Boris:   
oh....when copy/pasting. Aux paste can enable 3D mode on switch function if you had super expo on switch! Watch out for that!  

TODO - edit CLI commands   

# 2.7.1 - 

Patch 1 (2.7.1 notes):

- Fixed Luxfloat drift
- F1 boards defaulted to 1khz

Note: If you fly rewrite no need for update. Also updating doesn't need reset configuration.

# 2.7.0 -  

2.7 release

- Fixed MPU6000 SPI slow speed for some targets
- Fully working SPRACINGF3 Target
- Enable MPU6000 and MPU9250 for Colibri Race Target
- Many code cleanups and optimalisations
- Remove MW23 Pid controller (use rewrite or luxfloat)
- Fix for Dterm scaling in luxfloat. It is now fully matching rewrite. Luxfloat users from 2.6.1 need to multiply their D by 2 to get the same scaling
- Added PID configuration to blackbox header and other configuration parameters
- Added flightmode events to blackbox logging
- Added Optional Super Expo for yaw (CLI: super_expo_factor_yaw, super_expo_yaw)
- Iterm reset converted to Iterm limiter in super expo mode
- Added Optional Iterm reset option even without super expo (CLI: iterm_always_reset)
- Enabled TPA also for Yaw axis
- Increased default min_check value due to many misunderstandings for beginners (lower min_check still recommended)
- Added more debugging options
- Many code optimizations
- Optimizations for offline testing
- Change filter cutoff configurations to integers
- Dynamic PID Implementation (P accelerator)
- Added Task Page for OLED display
- Slightly improved biquad coefficients precision
- Fix for out of order PPM ISR
- Increased configurable range for yaw_p_limit
- Improved rc_expo step resolution by factor 5
- Added support for unsynced motor update speeds for fast PWM protocols up to 32k (CLI: unsynced_fast_pwm)
- Added new way of configuring fast PWM protocols (CLI: fast_pwm_protocol (ONESHOT125, ONESHOT42, MULTISHOT))        
**Yes this replaced "use MULTISHOT" and "use ONESHOT42"**
- Slowed down CPU for F1 targets back to 72Mhz to have better motor timing support. Naze32 may have more difficulties running 4k.
- Configurable Iterm reset offset (CLI: iterm_reset_offset)

NOTE
In this version the filters are optimised to provide the best possible flight characteristics. This means that some less agressive filtering is used. In case your setup is too noisy you need to adjust the filters. Here are some of the recommendations. The more filtering you use the less noise will be let into the system, but that will reduce the overall responsitivity of the pid controller and provide less stability in prop wash scenarios for example.

Default / Optimal flight performance:  
gyro_lowpass = 100  
dterm_lowpass = 110  
  
Slightly noisy setup:  
gyro_lowpass = 80  
dterm_lowpass = 100  
  
Very noisy setup  
gyro_lowpass = 50  
dterm_lowpass = 100  
  
2.6.1 defaults:  
gyro_lowpass = 80  
dterm_lowpass = 70  
  
Also some of the KISS24 users reported they removed some low throttle oscillations with setting gyro_lowpass to 50
  
# 2.6.1 - 

Release 2.6.1

- Very long waited VCP passthrough thanks to 4712! (You do need latest blhelisuite! http://www.mediafire.com/download/9bwzbfzmfpv7j4i/BLHeliSuiteBeta144051.zip)
- 4 way interface passthrough, which also works for simonk bootloader through blhelisuite
- Remove old simonk passthrough
- Cli bugfix for configuring Yaw lpf
- Fix low throttle arming for 3D mode on switch.
- Fix for re-arming when calibration set to be prior arming (only for switch armers due to safety reasons)
- Fix IBUS to have 10 channels
- Add frsky average voltage on A4
- Fix for transponder for SPRACINGF3MINI target
- Luxfloat now has exact same rate and PID scaling like rewrite. The defaults are now same. Makes switching between these 2 easier. Luxfloat users need to readjust rates and perhaps slightly their PID's. See wiki for more info
- Added new targets SPRACINGF3EVO and DOGE 
- Fix for uninitialized averageSum
- New Horizon default sensitivity (Level D is now horizon sensitivity for luxfloat and rewrite)

# 2.6.0 - 

Pre-release 2.6

- Improved performance of roll / yaw mixing to fpv cam feature
- Added vbat_hysteresis configurable parameter for vbat alarms
- Improved cli dump. "dump" = print all current active config, "dump all" = dump full config (all profiles and rates). Also no more pauses needed after pasting
- New additional esc passthough support added for Simonk bootloaders and blheli (example for esc #1: escprog sk 1 or escprog bl 1 for blheli) See wiki for explaination, which will be added soon
- Dterm filtering improved. Enabled by default. Dterm can now even be increased without fear for noise
- Luxfloat pids changed to integer and increased in configurable range. All pid controllers now share same PID bank. Luxfloat pids are now scaled to be close to rewrite. See wiki for conversion.
- Slight increase in rewrite yaw rate to match roll and pitch better. rate of 100 is now equal to 80 for example
- Acro Plus feature is depricated and replaced by Super Expo feature, which works differently. The feature is derived from multiwii and only applied to luxfloat and rewrite. super_expo_factor determines the super rate curve
- New optimized defaults
- Additional thresholds for Iterm reset in super expo mode added
- Fixed gyro calibration on arming features. Arming won't happen before gyro finished calibration
- Changed some parameter naming conventions in cli (mainly to prevent some copy paste behaviour and to have more clear naming)
- Improved yaw noise filtering
- Removed D on yaw from all pid controllers. This saves the pid controller some calculations. Dterm on yaw isnt really needed

# 2.5.4 - 
2.5 Patch 4

- Improved gyro calibration on faster looptimes

- Added option for gyro calibration prior arming. Gyro calibrates only prior the first arm sequence (set gyro_cal_on_first_arm = ON)

- Failsafe buzzer now only beeps when copter was armed at least once

- Iterm scaling improved on MW23

- COLIBRI_RACE added Airmode status in CORE PRO

- Fixed Beeper dump bug

- All profiles added to dump in cli

- Better configurable range for PIDs in configurator for Luxfloat. The 'P' and 'D' Terms in Luxfloat are now shown as 4 times higher to allow better resolution when tuning. The actual PID scaling stays the same and can be seen in the CLI.

- New luxfloat Yaw default

- Added 3D mode on switch (If you want to arm in non 3D mode your ESC range needs to be symmetric)

Notes from ßF thread posts:

Q: ..will 2.5.4 erase the settings I have now on 2.5.3? A: No it keeps your settings. Most patches dont require erasing.
Q: Off to test and tune! 2.5.4! A: No need for retune. It should fly the same. MW23 is the only one feel better locked in on same tune 

# 2.5.3 - 
2.5 patch 3

- yaw_p_limit applied to all PID controllers. Before it was only applied to MW23. Changed default to 300. Removed old dynamic yaw limiter

Note that upgrading from previous versions and/or pasting cli dump will overwite the default. Therefore I recommend using set yaw_p_limit = 300 for best results

# 2.5.2 - 
2.5 Patch 2

- Added piroutte protection during hard yaw manouvres. Yaw should not dominate over roll and pitch in some scenarios
- New 1000 looptime default (gyro_sync_denom = 8 and gyro_lpf = off)


# 2.5.1 - 
2.5 Patch 1

- Restored original min_check scaling. Both stick armer's and switch armer's have same throttle range.
- Default min_check lowered to 1040 (make sure your radio can go below 1040 to be able to arm)
- SPRACINGF3MINI cleanflight sync. Some timer cleanup (possible PPM fix with oneshot)
- Fix for ON_USB buzzer (works only on some targets)

# 2.5.0-RC7 Release 2.5
- reworked mixer saturation (smooth PID reduction)
- Should be the final 2.5.0 release

# 2.5.0-RC6
- Oneshot protocols always synced to pid loop now. No fixed motor rate available for oneshot125, oneshot42 or multishot, which didn't make sense anyway. When using 8k cycletime on oneshot125 PID loop / motor update will be limited to 2.6k to keep it safe. When using multishot or oneshot42 it will be increased. 
- Added autobind for spektrum on boot 
- Improved CPU usage by removing MOTOR task 
- Fixed BST

# 2.5.0-RC5
- Fixed non working blackbox and accelerometer (also model in configurator) 
- More efficient scheduler 
- Added attitude task 
- Fixed arming issues for stick armers 
- Fixed broken transponder for SPF3MINI 
- Added experimental gyro_lpf value 
- Changed some defaults

# 2.5.0-RC4
- Reworked motor task into a separate task within the scheduler 
- It will automatically choose the most optimal motor update rate. 
- Overclocked F1 targets except of CC3D 
- Fixed timing scaling on overclocked targets 
- Auto Config updated 
- Fixed broken servos 
- Note: BlackBox and a few other features including the 'quad' graphic in the config GUI got disabled accidentally in this code. Stick Arming also broken. Fixed in RC5

# 2.5.0-RC3
- Fixed issue with fast_pwm rates above 4k 
- renamed the feature to set forced_motor_pwm = . forced_motor_pwm applies to all ESC protocols....oneshot125, oneshot42 and multishot. 
- Auto configuration via looptime will choose a highest safest fixed value. 
- Also chirping bug on idle with fixed pwm rate is resolved

# 2.5.0-RC2
- Improved performance on all targets
- Automatic fast_pwm calculation on faster looptimes
- Removed emf_avoidance on F1 targets
- Improved Acc readings on faster looptimes
- Fix for Dodo jitter on 4k (baro should be disabled there and is now automatically)

# 2.5.0-RC1
Pre-Release RC1
- Scheduler rework for more spread processing and faster looptimes
- Decreased motor jitter due to inverted order for main task
- Removed rcCommand throttle to min_check dependancy. Throttle is not anymore constrained to min_check
- Without airmode less low throttle authority
- Rateprofile cleanup
- Restored original Mode order
- Configurable VFAS cell voltage / battery voltage (set "frsky_vfas_cell_voltage= ON/OFF")
- Better D averaging method
- Oneshot42 Implementation (When oneshot feature enabled type "set use_oneshot42 = ON")
- Multishot Implementation (When oneshot125 feature enabled type "set use_multishot = ON")
- Fast PWM support for all fast PWM famillies oneshot125, oneshot42, multishot
- Remove Iterm reset from AIRMODE (Moved to ACROPLUS)
- Fixed cli dump hickup
- Improved Auto settings configuration with configurator looptime parameter
- Enabled Faster looptimes on all targets due to spread processing. SPI targets can now go up to 8khz cycletime with motor update speed @4khz
- Improved MW23 Pid controller (fixed scaling to cycletime)
- Added SBUS inversion cli command ("set sbus_inversion = ON" ONLY ON F3 TARGETS)
- Increased max_aux_channel defaults to 6
- Reworked task manager (cli option "tasks"). It shows more information about processes

NOTE: Use configurator for configuring looptimes. F1/F3 targets can easily use looptime 250 (4KHZ), motor update speed will be 2khz. F3 Targets like LUX_RACE, Cyclone and COLIBRI_RACE can use 8khz (125us looptime)

When upgrading from 2.4.1 and pasting dump FAILSAFE mode can be enabled on arm. re-check and reconfigure your Modes tab

# 2.4.1-RC3
- Fix oneshot kills RX_PPM

# 2.4.1-RC2
- Fixed typo in cli for acro_plus_offset

# 2.4.1-RC1
- Release 2.4 patch 1 pre-release:


- Replaced many of float math by fixed point math. This will especially improve performance on F1 boards
- Improved Acro Plus
- Added acro_plus_offset cli parameter. Tells the percentage stick input where acro_plus will start working and will have a smooth transition from there on
- Added back PID3 (for experimental purposes watch out with defaulf pids on this one)
- Added more options for Dterm averaging (dterm_average_count range: 2-12)
- Improved filtering for accelerometer
- Configurable max AUX channels. (cli option: max_aux_channels)
- Some cleanups
- Fixed PPM + oneshot125 in SPF3MINI
- Better anti spool up protection without AIRMODE and smoother motors on Idle
- Added rateprofiles again
- Removed GTUNE on all targets due to a lot of inconsistancy in results. This also provides more flash space on 128k targets
- Fixed incorrect cycletime reporting. It turns out that the cycletime reported in configurator was not the real one since 2.2
- Much more improved jitter. Due to wrong cycleTime reporting the jitter numbers were underrated and there was much more jitter. Due to new jitter buffering the cycletimes are rock solid with healthy CPU state. This may also provide better tuning results on fast refresh rates.

NOTE! Due to removal of GTUNE loading old settings will result in swapping AIRMODE switch with ACROPLUS....make sure you check that in the modes tab!

# 2.4.0-RC9
- More improved battery voltage filtering 
- Fixed cli AUX / servo dump - Fixed 
- Fixed gyroADC overflow what could cause infinite flips and rolls 
- Fixed accelerometer overflow 
- Fixed some alienwii Led issues
- Note: Changing "rate profile selection" on AUX switches is removed.

# 2.4.0-RC8
- Fixed Battery voltage
- Note: AUX settings not shown in CLI with 'dump'. Type 'aux' to see these.

# 2.4.0-RC7
- Fixed Blheli ESC passthrough 
- Fixed LED_STRIP feature 
- minor cleanups 
- RACE targets should now always flash fine

# 2.4.0-RC6
- Fixed SPFRACINGMINI 
- added failsafe_procedure 
- fixed Luxfloat iterm calculation 
- Removed Baro for CC3D to fit the code 
- Added Iterm windup protection during failsafe and unarmed conditions 
- Disable arming in negative direction for 3D

# 2.4.0-RC5
- Fixed 3D transitions (now tested) 
- Added Jeti Exbus support 
- Fixed CC3D PPM bug 
- New level defaults

# 2.4.0-RC4
- Broken targets....went to RC5 to prevent confusion

# 2.4.0-RC3
- Fixed RACE targets
- fixed configurator from saving wrong looptime settings

# 2.4.0-RC2
Release candidate updates:
- Fixed 3D inverted throttle when arming

KNOWN ISSUES IN RC2:
- LUX and COLIBRI_RACE target not working

# 2.4.0-RC1
New Major Release:

Pre release of 2.4.0:

- Added new targets SPRACINGF3MINI, ALIENFLIGHTF1, ALIENFLIGHTF3
- Major sync/catch up with Cleanflight
- SD card support for SPRACINGF3MINI target with DMA usage for blackbox
- MSP version updated to support all current Configurator features
- Added serial buffering for faster VCP and UART communication
- LUX target moved 1wire to USART3
- Update cli parameters (Will be updated on Wiki). Perform a dump to see the differences.
- Add flexible gyro speed sampling when gyro_lpf set to off. additional parameter is gyro_sync_denom. To get 2khz       support set it to 4. denom is always a multiplier to 125us=8khz. 4 means 125*4=500us(2khz). Dont forget to check CPU usage when playing with this value.
- Gyro update speed can now be configured through configurator. For 2khz just set to looptime 500 and it will automatically set the correct values for your board. I recommend configuring this through configurator. Bare in mind that acc, baro and mag will automatically be disabled on F1 boards when setting on faster speeds than 1khz (1000us)
- Many SPI fixes
- Reduced profiles from 3 to 2. Moved many parameters to master profile. Only PID's, rates and few others are now part of profile. No need to reconfigure everything when using 2nd profile
- Added PID scaling to vbat voltage. "set vbat_pid_compensation = ON". It uses maximum cell voltage as an offset. Tune your quad with a full lipo and your pids will be scaled up to 25% when voltage gets lower.
- Fixed a bug where USB error interrupt would saturate CPU and caused a board not be able to arm on some targets
- Mixer rework. Old airmode mixer is now default mixer. Air mode still exists as without airmode there is no Iterm on 0 throttle and there is also no Iterm scaling in acro
- Added configurabe airmode saturation_limit. Default value of 50 means that airmode will try to compensate at it's best till 50% saturation. 0 means always maximum stabilisation and 100 always limited. 0 is like version 2.3.3 and 2.3.5 and 100 would act same like pre 2.3.3 and like in 2.3.5.
- Improved 3D transition from negative to deadband.
- Status option will now display CPU usage in percentage instead of load
- More inflight adjustments now possible
- Slightly different anti windup behaviour in airmode.
- MOTOR_STOP is now overruled by AIRMODE! It acts as a Idle Up switch now. There is no point of using motor stop in airmode
- Updates to build environment. Version number added to filename
- Acro plus is on a mode.....you can enable it by switch
- ACRO PLUS increased sensitivity
- Added Jeti Exbus Support
- Fixed PPM glitch bug on CC3D (PPM pin moved from 3 to 8)
- Added failsafe_procedure configurable through the new configurator

NOTES:
- Pre-Releases are not yet fully stable releases, but are released to get valuable feedback
- Users of Vortex pro should get the corresponding OSD firmware!
- Use Cleanflight Configurator 1.2 (recommended)

# 2.3.5
Airmode Saturation Behaviour (Same like 2.1.6 and 2.2.0. Some setups cannot deal with aggressive corrections. This version only gives solution for those who have issues with double rolls etc. Basically the mixer mechanism is changed like in 2.1.6 and 2.2.0. It might feel a bit softer than 2.3.4 in hard manouvres, but that has been proven to work on all setups in the past.

"...spazzing out is a result of not enough power to get desired correction. 
I restored the original mixer behaviour:...
When mixer comes to the conclusion that desired PIDsum cannot be achived with the current motor powers it has to do something else than just mixing. This is what I mean with saturation scenarios."

# 2.3.4
Softer D approach as default baseline. Goal is to eliminate more D noise which should allow for higher D values for noisier quads. Rc_smoothing disabled by default as the new gyro delta approach should smooth out D anyway. See 2.3.3 for important details.

# 2.3.3
A lot of improvements and changes in this release. 3D airmode is much improved. Use set gyro_lpf = off to allow for 2khz mode. On F1 targets remember to also disable mag/baro/acc. **IMPORTANT: Lower your D values (to 0-5) when first enabling 2khz mode and check motor temps frequently! Some users have reported cooked motors as a result of D values that are too high.**

#2.1.6
A stable release. Use this if you are just getting started with betaflight and do not want 2khz mode or 3D airmode.