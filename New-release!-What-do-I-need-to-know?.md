This page is meant to provide a more in depth explanation of the changelog for each release so that users can understand the consequences of the changes made. Please remember to read the changelog for each release first.

# 2.5.0-RC2

- Improved performance on all targets. 
- Automatic fast_pwm calculation on faster looptimes. 
- Removed emf_avoidance on F1 targets. 
- Improved Acc readings on faster looptimes. 
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