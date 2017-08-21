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


### From Boris:  
Hi Guys. I hope everyone is fine.

We finally have a release candidate on schedule like promised!   
https://github.com/betaflight/betaflight/releases

## Betaflight 3.2.0 RC3 Pre-release

### New:

- Full F7 support (@sambas @blckmn )
- SITL simulator support (@cs8425)
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

### Experimental - use with caution

- Added experimental crash detection and recovery (@martinbudden)
- Added experimental CPU overclock options for F4
- Fixed full throttle drift when airmode disabled
- FrSky SPI RX support

### Fixes:

- Fix GPS serial overflow (@mikeller)
- Fixed destabilisation on full throttle when Airmode disabled

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


This is the current signal path in ascii art:

gyro -> dynamicNotch -> notch1 -> notch2 -> lpf -> P term                                           
-> motors    
                                               \-> D term -> notchD -> lpfD -> setpointRelax&Weight /    

### Crash Recovery

TO DO - fill in CLI commands and what they do.

TCHTHSKY Posted in Boris' thread-
[Here's the code for it. It has comments: ](https://github.com/kc10kevin/betaflight/blob/master/src/main/flight/pid.c)   
[And here is the feature request: ](https://github.com/betaflight/betaflight/issues/2731)   
[And here's this: ](http://stackissue.com/betaflight/betaflight/added-experimental-crash-detection-and-recovery-2783.html)   
[Here's another from earlier in this thread:](https://www.rcgroups.com/forums/showpost.php?p=37951070&postcount=49982)   

### Turtle Mode  

TO DO - Describe what this does, how to setup and which ESC firmware supports this.


## CLI command changes in 3.2

TO DO - Compare a CLI dump from 3.1.x to 3.2 and list all differences.

### Removed CLI Commands

TO DO - List all CLI comands that were in V3.1.x that are not in V3.2.

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
#### set crash_dthreshold = 50
[0..2000]
#### set crash_gthreshold = 400
[0..2000]
#### set crash_recovery = OFF
[OFF, ON, BEEP]
#### set crash_recovery_angle = 10
[0..30]
#### set crash_recovery_rate = 100
[0..255]
#### set crash_setpoint_threshold = 350
[0..2000]
#### set crash_time = 500
[0..500]
#### set horizon_tilt_effect = 75
[0..250]
#### set horizon_tilt_expert_mode = OFF
[OFF, ON]

### Name changes

TO DO - add to this list all CLI commands that changed names
 
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
