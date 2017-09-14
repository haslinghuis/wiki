IMPORTANT ANNOUNCEMENT

## Betaflight v3.2 will be the last version to include STM32F1 based flight controllers. From v3.3 onwards support for those flight controllers will no longer be provided, this includes the NAZE, CC3D (original), ALIENFLIGHTF1 and MICROSCISKY and their clones.

If you have any questions or concerns please advise in Boris' BetaFlight thread. Providing notice here so that there is plenty of time to prepare. v3.2 will be released in October, whilst v3.3 won't be until next year.

You can see some information here : https://github.com/betaflight/betaflight/projects/2 however it doesn't look complete.

From Boris (30 June 2017):   
Well release candidate is around the corner.
Basically a lot of fundamental changes. Not much of those will affect the user, but there are also quite some new features as well.   
The major complain about Betaflight was too frequent updates in the past. So we slowed that down. Not having the pressure for quick releases gives some time for thinking and researching new things.  

Current V3.2 files are here: https://ci.betaflight.tech/job/Betaflight/lastSuccessfulBuild/artifact/obj/ 
or https://betaflight.qmd.cl/

### Note: When discussing this Version in the Forums (Boris' BetaFlight thread)   
## Please State: Version and Build Number.

### If having a problem check [ISSUES](https://github.com/betaflight/betaflight/issues) to see if it have been reported and a possible work around.

### From Boris:  
Hi Guys. I hope everyone is fine.

We finally have a release candidate on schedule like promised!   
https://github.com/betaflight/betaflight/releases

## Betaflight 3.2.0 RC5 Pre-release

### New:

- Full F7 support (@sambas @blckmn )
- [SITL](http://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html) simulator support (@cs8425)
- Added crosshairs to CMS (@lostcontrol)
- Improved ITerm windup handling for tricopter (@martinbudden)
- Added SP RACING F3 OSD/PDB suport (@hydra)
- Added horizon_tilt_effect command (@ethomas997)
- Improved scheduler efficiency (@lilcw)
- Increased motor output resolution (@borisbstyle)
- Improved pwm timer precision (@blckmn)
- Added new motor protocol Proshot1000 (@TonyBazz)
- Improved configuration architecture (PG implementation @martinbudden , @ledvinap)
- OSD improvements
- New target support
- Added Automatic Notch filter based on noise frequency (@rav-rav , @martinbudden)
- Fix/enable disabling of rc smoothing in level modes
- Flip inverted quad on ground (anti-turtle mode) (@brycedjohnson)
- Improved blackbox storage to be more compact. Allows recording at higher rates and/or longer logs on flash storage (@martinbudden)
- Camera control
- TBS compatible LED frequency indicator
- beeper / OSD / CLI indication of reason for not arming;
- Added Iterm limit to prevent strong accumulations

### Experimental - use with caution

- Added experimental crash detection and recovery (@martinbudden)
- Added experimental CPU overclock options for F4

### Fixes:

- Fix GPS serial overflow (@mikeller)
- Fixed destabilisation on full throttle when Airmode disabled
- Less chance on yaw spins with Iterm limit

### RC2 Changes:  
- Fix for new blackbox denominators
- Improved ACC scaling in some cases  

### RC3 Changes
- MATEKF405 Enable follow me spektrum binding
- Fix for blackbox p_denom
- ROM savings
- Add missing AHI sidebar in cli
- Check for notch 0 in filter init
- rework on pin timer mapping
- Fix buzzer on alienwhoop v2.0
- Spektrum Telemtry fix for FlightPack Capacity
- Fix Acc reporting for some gyros in configurator

### RC4 Changes
- Added Experimental Slew filter
- Fixed lockup on MSP for dterm filter change
- Improved gyro debug logging
- Validate features when not supported
- Experimental Yaw overflow handling
- Added OSD warning for battery not full
- Fix cli settings for OSD timers
- Added Iterm limit to prevent strong accumulations
- Reset iterm after crash for crash detection

### RC4 Bugs (see the Issues and Pull requests in Github for details)
- gyro debug and notch debug doesnt work. No data is displaying in analyzer. Fixed in Build #179. 
- various [Issues](https://github.com/betaflight/betaflight/issues/4001) with DSHOT1200
- NAZE target not able to log data to Black Box
- Gyro Cal beeps [issue 4107](https://github.com/betaflight/betaflight/issues/4107)
- effectively made the setpoint weight behave as if it was set to zero (no matter what you set it to) if the setpoint transition was at the default 1.0. Fixed in RC5.

### RC5 Changes
- Default setpoint weight has been changed to 0. This should result in flight behaviour identical to previous 3.2 RC's if the user left the relax ratio at the default of 1, because relax ratio of 1 was disabling setpoint weight in those RC's. This has been fixed in RC5, so that D weight is active even if relax is 1. Users wanting D weight behaviour like 3.1 should set weight to 0.6.
- Removed experimental Slew filter
- Disabled beeping on gyro calibration during boot
- Added gyro overflow protection for some newer gyros (ICMxxx gyro series is known to show inversion due to overflow above +/-2000deg/sec)
- Added internal resistance adjustment for camera control
- Show MSP version in cli

### Other Features:   

- Spektrum Satellite Bind for 3.2  
https://github.com/betaflight/betaflight/wiki/Spektrum-Satellite-Bind-for-3.2

- Reconfigurable Barometer for 3.2  
https://github.com/betaflight/betaflight/wiki/Barometer-Configuration-(3.2)

## Black Box Viewer
https://github.com/betaflight/blackbox-log-viewer/commits/master

## Instructions for Upgrading - 

DO NOT use Copy/Paste of CLI commands since many have changed- either names or new ones.
Use the latest Configurator to setup then the CLI Tab and manually set.

## New features and changes of how old feature work

### Switching from 'tlm_inversion' to 'tlm_inverted'

With the change of the configuration parameter `tlm_inversion` to the parameter `tlm_inverted`, the scope and function of the parameter have changed:

- `tlm_inverted` applies to **all** telemetry protocols;
- `tlm_inverted = on` means that telemetry is expected to be inverted **compared to what it is for the selected protocol** (i.e. when using SmartPort, `tlm_inverted = off` means that the flight controller expects the telemetry signal to be inverted serial, since this is the default for SmartPort).

_This means that for all protocols, if unmodified hardware is used `tlm_inverted = off` is most likely the correct setting._

(Also, note that this only works for F3 / F7 based boards, or for F4 boards on ports with switchable external inverters.)

### Dynamic Filters

TO DO - fill in CLI commands and what they do.

NOTE: F1 boards can't handle dynamic filters.. no space, no power for it.  

This is the current signal path in ascii art:

gyro -> dynamicNotch -> notch1 -> notch2 -> lpf -> P term                                           
-> motors    
                                               \-> D term -> notchD -> lpfD -> setpointRelax&Weight /    

Dynamic filter runs in the gyro loop, so PID loop freq doesn't matter. 
Don't run 8k on F3 processors, use 4/4 on those. F4 / F7 should run fine on higher freq. 

mjbudden posted on Boris' thread regarding the gyro/PID loop speed and Dynamic Filtering: 
Official recommendation from myself and r.a.v. (who between us wrote the code) is _not to use above 4k/4k on F3 processors if using dynamic filtering_. When running 8k gyro you have 125 microseconds in each loop and it is simply not enough time to run the calculations. So the PID loop will run slower than 8k anyway. There may be no outward signs of problems, but there will be jitter in the PID loops and that can potentially cause problems.

### Crash Recovery

TO DO - fill in CLI commands and what they do.

Briefly the mode works like this:  
1. A crash is detected if `crash_dthreshold` and `crash_gthreshold` are exceded.  
2. Once a crash is detected, the craft tries to level itself, ignoring RC input on roll and pitch axes.  
3. Craft continues to try and level itself until either:  
1. `crash_time` milliseconds after the crash, or  
2. the craft angle is less than `crash_recovery_angle` degrees on both the roll and pitch axes and the craft gyro rate is less that `crash_recovery_rate` degrees/second on both roll and pitch axes.  
4. For testing purposes crash recovery can be enabled by turning the beeper on (this will be removed/changed if crash recovery proves successful).    

TCHTHSKY Posted in Boris' thread-
[Here's the code for it. It has comments: ](https://github.com/kc10kevin/betaflight/blob/master/src/main/flight/pid.c)   
[And here is the feature request: ](https://github.com/betaflight/betaflight/issues/2731)   
[And here's this: ](http://stackissue.com/betaflight/betaflight/added-experimental-crash-detection-and-recovery-2783.html)   
[Here's another from earlier in this thread:](https://www.rcgroups.com/forums/showpost.php?p=37951070&postcount=49982)   

#### Important note from mjbudden:  
The crash recovery routine does not currently attempt to limit yaw spin - it just tried to level the quad. I did not anticipate the "yaw spin to the moon" problem when I wrote the crash recovery routine. I'm now working on a solution. See PR github.com/betaflight/betaflight/pull/3909 in github for the current discussion on the subject. Feel free to contribute to the discussion. Also black box logs of "yaw spins to the moon" are very welcome and will help analyze and solve the problem.    
From Yamaford:  
Just to clear this up for others that may read this wrong, I don't think your crash recovery code is the culprit as the crazy "YSTTM" (Yaw spin to the moon) is happening with or without acc or crash recovery enabled right?   
From mjbudden:   
Correct. Yaw spin to the moon happens with or without crash recovery enabled and is not cause by crash recovery code.  
There is a new PR that tries to fix this, see [PR 3909](https://github.com/betaflight/betaflight/pull/3909). If you could test this and provide blackbox logs it would be most useful.   

### "flip after crash mode" (was called Turtle Mode)  

TO DO - Describe what this does, how to setup and which ESC firmware supports this.

Description by brycej:  
In its current form (as of last week, 8aug17, or so). You need to flick the mode switch and then arm. Throttle and yaw sticks don't do anything. Pitch and roll will control which props are spinning based on stick deflection. With the pitch and roll centered no props are moving.  
Take a good guess on the side with free props and try and flip it over. If it isn't flipping, try a different pitch or roll. Don't just jam the stick over and leave it if it is stuck. I haven't burned up any escs in quite a bit of testing, but I'm just someone will manage it with some effort....  
Disarm after flipped. Undo flipaftercrash mode and rearm to fly away.  
Other tips: Has worked well to get me out of trees.

Try it with props off on the bench first. 16.63 from official blheli hasn't some issues reversing all the motors because it is a little more stringent then 16.67 from the unofficial blheli fork. Newer betaflight versions have some fixes in to help that.  
Also, try it in short grass LOS so you can see how it works...  
Can't fix broken props or save you or if long grass, so doesn't always work.  

RCs 4 or below inverted the mixer and had all 4 props reversed and spinning. That works too but it is a little more messy on the ground and requires some finesse. 

### Experimental Slew filter- Removed in RC5
https://github.com/betaflight/betaflight/pull/3983  
More info here:  
https://github.com/betaflight/betaflight/pull/3909  
https://github.com/betaflight/betaflight/issues/3959#issuecomment-326430286
https://github.com/betaflight/betaflight/pull/3950  
https://github.com/betaflight/betaflight/issues/3893  

## CLI command changes in 3.2

### Removed CLI Commands

feature -VBAT  
feature -FAILSAFE  
feature -CURRENT_METER  
feature -BLACKBOX  
feature -SDCARD  
feature -VTX  

set align_mag   
set bat_detect_thresh   
set blackbox_rate_num   
set fixedwing_althold_dir  
set frsky_vfas_cell_voltage   
set gyro_use_32khz  
set mwii_ibat_output  
set servo_lowpass  
  

### New CLI commands

TO DO - list all new CLI commands with a description of what they do and how to use them (links are good) and what the Default and optional values are.

#### feature -DYNAMIC_FILTER
#### beeper BLACKBOX_ERASE

GLOBAL SET  
#### set beeper_frequency = 0
[0..16000]  

#### set blackbox_record_acc = ON
[OFF..ON]  

#### set camera_control_key_delay = 150
[100..500]

#### set camera_control_mode = HARDWARE_PWM
[HARDWARE_PWM, SOFTWARE_PWM, DAC]

#### set camera_control_ref_voltage = 330
[100..400]

#### set dashboard_i2c_addr = 60
[8..119]
#### set dashboard_i2c_bus = 1
[0..2]
#### set esc_sensor_halfduplex = OFF
[OFF, ON]

#### set ibatv_offset = 0
[-16000..16000]
#### set ibatv_scale = 0
[-16000..16000]
#### set led_inversion = 0
[0..7]
#### set motor_pwm_inversion = OFF
[OFF, ON]
#### set report_cell_voltage = OFF
[OFF, ON]
#### set vbat_detect_cell_voltage = 30
[10..50]
#### set motor_pwm_protocol = ONESHOT125
[OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED, DSHOT150, DSHOT300, DSHOT600, DSHOT1200, PROSHOT1000]

Per PROFILE:   
#### set crash_delay = 0
[0..500]
#### set crash_dthreshold = 50 :degrees/second/second
[0..2000]
dterm crash value, zero (off) by default, set to at least 1 to enable crash detection  
#### set crash_gthreshold = 400 :degrees/second
[0..2000]
gyro crash value  
#### set crash_recovery = OFF
[OFF, ON, BEEP]
#### set crash_recovery_angle = 10 :degrees
[0..30]
#### set crash_recovery_rate = 100 :degrees/second
[0..255]
#### set crash_setpoint_threshold = 350 :degrees/second
[0..2000]
#### set crash_time = 500 :ms
[0..500]
#### set horizon_tilt_effect = 75
[0..250]
#### set horizon_tilt_expert_mode = OFF
[OFF, ON]

### Name changes

#### yaw_motor_direction [1, -1] ==> yaw_motors_reversed [OFF, ON]
#### sport_halfduplex ==> set tlm_halfduplex
#### tlm_inversion' ==> tlm_inverted
#### spektrum_sat_bind_autorst ==> spektrum_sat_bind_autoreset  
#### dfu ==> bl
#### current_meter_type = ADC ==> current_meter = VIRTUAL
#### d_lowpass ==> dterm_lowpass  
#### d_lowpass_type ==> dterm_lowpass_type  
#### d_notch_cut ==> dterm_notch_cutoff  
#### d_notch_hz ==> dterm_notch_hz  
#### digital_idle_percent [7.00] ==> dshot_idle_value [450]
#### yaw_accel_limit ==> acc_limit_yaw  
#### yaw_control_direction [1] ==> yaw_control_reversed [OFF]
#### anti_gravity_thresh ==> anti_gravity_threshold  
#### battery_meter_type ==> battery_meter
#### blackbox_rate_denom [4] ==> blackbox_p_denom [32]
#### ibat_offset [0] ==> ibata_offset [0]
#### ibat_scale [400] ==> ibata_scale [400]


## Changes in the New Configurator GUI (V3.2.2)

### Receiver Tab  
 Added "Stick Min", "Stick Center" & "Stick Max" ---these are simply MIN_CHECK, MID_RC, MAX_CHECK.
