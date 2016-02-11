This page is meant to provide a more in depth explanation of the changelog for each release so that users can understand the consequences of the changes made. Please remember to read the changelog for each release first.
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