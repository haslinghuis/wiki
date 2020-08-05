### The below presets are _**by the community, for the community**_.  You will see the BF version the preset is targeted toward and the pilot's name who published their recommended preset(s).  We encourage community members to provide their own presets on this wiki page.  To do so, you simply need a Github account.  Enjoy!

### IMPORTANT: These settings are NOT provided or endorsed by the Betaflight project. They are examples that others have found helpful for their particular quad. THEY MAY NOT BE SUITABLE FOR YOUR QUAD! A better use might be to examine similar configurations to yours and get ideas on possible tuning directions rather than blindly copy/pasting someone else's settings. Always test carefully and safely.

<p> To use, simply copy and paste the preset CLI commands into the Betaflight CLI tab.  After the paste, type "save" and hit [enter].  That will load the settings of the preset which you can see (mostly) through the GUI tabs.</p>

<br/><br/>

# BF 4.2.X

Betaflight 4.2.X with configurator 10.7 introduces significant and helpful features for tuning, many of which are highly recommended.

These features include VBat Sag Compensation, Feed Forward Interpolation, fixed I-Term Relax operation on Setpoint, and looptime improvements that allow for more stable filtering calculations. 
Existing features can also be leveraged to produce better performing and more robust tunes, including DMin/Boost Gain & Advance, TPA, and Thrust Linearization

VBat Sag Compensation is a feature which aims to produce consistent motor response across the entire flyable voltage profile of a battery pack, and does this by slightly reducing peak motor outputs from the mixer at high voltage, and increases these values as battery sag comes into play.  This is a highly recommended feature, although if you rely on 'feeling sag' as an indicator to land shortly, or are concerned about damaging  batteries, particularly 6-Cell batteries on relatively efficient rigs, you can use values lower than 100 to receive most of the benefits, and also receive a small amount of extra initial 'punch' on fully charged batteries with values such as vbat_sag_comp = 70.

Feed Forward (FF) Interpolation has been improved, which uses a trailing average of 2-4 radio control setpoint samples to set feed-forward input values (which are then multiplied by the FF Gains).  For noisier or less consistent RC signals (e.g. FrSky R9), using ff_interpolate_sp = 4 is a significant improvement, while more consistent signals (CRSFShot, AFHDS2A-IBUS, Futaba FASSTer) produce smooth enough traces to use ff_interpolate_sp = 2 (default) values with lower automatic RC_Smoothness values (around 8 is still stable enough for long range capable 7" craft).

I Term Relax now works correctly on Setpoint mode at values below 20.  Particularly for large craft, lower values result in reduction or complete elimination of I-Term driven bounceback on flips and rolls.

DMin/Boost tools such as the DMin-Gain and DMin-Advance can be leveraged to increase effective D gains during stick movements (dMin Advance mostly) and propwash (dMin Gain mostly).  Values of Gain = 44-55 and Advance values of 80-100 can result in much higher and more consistent D-Term response to damp P-term step responses, while still allowing for lower DMin values during normal flight that does not amplify higher frequency noise.

Thrust_Linear is intended to linearize aspects of thrust delivery (which is typically a quadratic response to throttle position) in order to produce more consistent PID response across the range of throttle inputs.  In practical implementations, it is most frequently used as a way to increase PID gains at low throttle to compensate for lower authority craft (low voltage 65mm and 3" lightweight quadcopters, as well as 6-8" quadcopters on smaller motors can benefit from values of thrust_linear from 20-30 in order to remove low-RPM bobbles and instability.  Quite often, best results are achieved by simultaneously increasing the TPA (Throttle PID Attenuation - most practically this is a throttle-DTerm-attenuation factor at default) value, and moving the breakpoint higher (e.g. TPA = 0.72-0.78, TPA_Breakpoint = 1270-1420).  This combined effect allows for boosted PID response at low throttle values, but does not result in excessive motor heat from amplified D gains at high throttle.

Preferred tuning methods can lean heavily on the Slider functionality in the 10.7 configurator, with many archetypes of copters being fairly quick to tune quickly with sliders based on quick information, following a standard procedure of determining the correct P:D gain ratio, incrementing the P&D Gains until oscillations or trilling sounds are observed (then reducing 1-2 clicks), then finally adjusting FeedForward through gain sliders and FeedForward Transition (which reduces effective feed forward gain from center point out to the specified value linearly) until desired stick feel is achieved.

Note: The D Ratio tuning slider in the 10.7 Configurator is different from previous versions - moving the slider to the right increases D gains while leaving the P gains as-is.  Values of 1.3 on the P:D Ratio slider produce gains of P≈D, while 1.0 produces a P:D ratio where D gains are roughly 0.8x of P gains.

Dynamic Notch Filter ranges are specified by minimum and maximum Hz - if you transport a tune from BF4.1.X to BF4.2, you will need to change the dyn_notch_max_hz value to an actual value in Hertz - recommend setting this value to dyn_notch_max_hz = 350 for most applications.

<br/><br/>
## Pilot: Tehllama

---
#### Racing Snippets 
##### 3" 1105 5500KV 3S, 3" 1408 4100KV 4S, 5" 2150KV 6S, 7" 2408 1622KV 6S
<details><summary>CLI Copy\Paste</summary>

```python
#Settings for All Quadcopters
set debug_mode = GYRO_SCALED
set iterm_relax = RPY
#-This is a somewhat odd setting
set vbat_pid_gain = OFF
set vbat_sag_compensation = 70
set yaw_lowpass_hz = 100

#LlamaRates - 360°/s on Roll/Pitch, 270°/s on Yaw
set roll_rc_rate = 72
set pitch_rc_rate = 72
set yaw_rc_rate = 54
set roll_expo = 0
set pitch_expo = 0
set yaw_expo = 0
set roll_srate = 60
set pitch_srate = 60
set yaw_srate = 60

#LaunchControl_Preferred
set launch_control_mode = PITCHONLY
set launch_trigger_allow_reset = OFF
set launch_angle_limit = 60

save

```
</details>

# BF 4.1.X
Betaflight 4.1.X with configurator 10.6 

Note: the P:D Ratio slider in the 10.6 Configurator is reversed from the later releases - in 10.6 moving the slider to right increases P gains while decreasing D gains, which is not the behavior in the 10.7 Configurator.  A value of 0.8 on the PD Balance slider produce gains of P≈D, while 1.0 produces a P:D ratio where D gains are roughly 0.8x of P gains.

There are added features, and a couple of noteworthy bugs present in 4.1.X.  

Setup for enabling bidirectional DShot features (RPM filtering, dynamic idle) is simpler in BF4.1.X, as is configuring RC Smoothing using the configurator parameters.  With utilizing RPM Notch filters for attenuation of motor noise bands, changes in lowpass filtering (particularly dynamic lowpass filtering) are typically required in order to achieve the substantial improvements possible with RPM filtering.

Feed Forward (FF) Interpolation has been introduced, which uses a trailing average of 2 radio control setpoint samples to set feed-forward input values (which are then multiplied by the FF Gains).  For noisier or less consistent RC signals (e.g. FrSky R9), using ff_interpolate_sp = averaged is an improvement, though BF4.2 and later allows for higher numbers of samples to be required (up to 4). 

The I-Term Relax mode in Setpoint does not achieve any effective values below 20 - for larger craft where lower iTermRelax values are needed or tuning strategies that rely on removing I term involvement to tune P/D Ratios and gain, shift this to Gyro (in 4.2 and later, you can use Setpoint again).

VBat PID Compensation is often helpful for tuning flights, and is a recommended setting when conducting blackbox logging, or if craft authority as battery voltage approaches sag points is important.  This has been effectively replaced by VBAT Sag compensation in BF4.2 releases.

Preferred tuning methods can lean heavily on the Slider functionality in the 10.6 configurator, with many archetypes of copters being fairly quick to tune quickly with sliders based on quick information, following a standard procedure of determining the correct P:D gain ratio, incrementing the P&D Gains until oscillations or trilling sounds are observed (then reducing 1-2 clicks), then finally adjusting FeedForward through gain sliders and FeedForward Transition (which reduces effective feed forward gain from center point out to the specified value linearly) until desired stick feel is achieved.  

Running higher Yaw P gains is entirely possible, although sliders do not allow this behavior - the defaults in BF4.2 (using the same effective PID gains) allow for running Yaw P Gains equal to Roll P gains, and works very well for most craft, so do not be concerned if during the tuning process higher Yaw P gains are required, you should proceed to increase those gains until matching Roll P gains without concerns.

# BF 4.0.X

<br/>

---
#### Back to 4.0.x Defaults
This will take your setup back to BF 4.0.x defaults
<br/>
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 150

set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 450
set gyro_lowpass_hz = 0
set gyro_lowpass_type = BiQUAD
set gyro_lowpass2_hz = 150
set gyro_lowpass2_type = PT1

set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass_type = BiQUAD
set dterm_lowpass_hz = 0
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 100

set dyn_notch_width_percent = 8 #Dual Dynamic Notches is as default; 8% spread from center to center.

#PID Gains Settings
set vbat_pid_gain = OFF
set anti_gravity_gain = 5000
set p_pitch = 46
set i_pitch = 70
set d_pitch = 38
set f_pitch = 75

set p_roll = 42
set i_roll = 60
set d_roll = 35
set f_roll = 70

set p_yaw = 35
set i_yaw = 100
set d_yaw = 0
set f_yaw = 0

set d_min_pitch = 20
set d_min_roll = 22
set d_min_boost_gain = 27
set d_min_advance = 20

set pidsum_limit = 500 #restricted to 50% by default

#For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = Setpoint
set iterm_relax_cutoff = 20

#TPA Settings (which is D-term only by default)
set tpa_rate = 50
set tpa_breakpoint = 1500
save

```
</details>

---
<br/><br/>
## Pilot: UAV TECH

<b>
**ALWAYS CHECK YOUR MOTOR TEMPS AFTER A SHORT 15 SECOND FORWARD FLIGHT** <br/><br/>
**WARNING:  If your mechanical/electrical issues are not addressed (most of the time those are the issues folks have), the below settings may require heavier filtering and lower PID gains (the BF default).**  Heavier filtering and lower PID gains can provide for cooler motors, but also has worse flight performance.<br/><br/>
</b>

A clean build is when a spectrograph of the RAW gyro noise trace (Debug_Mode=Gyro_Scaled) on a FULL & HARD flight looks like the below or better: <br/>
<a href="https://github.com/spatzengr/UAVtech-Resources/blob/master/Gyro_Raw%20Noise%20Profiles/Clean/Nova%20on%20BF4.0.png" target="_blank"><img src="https://github.com/spatzengr/UAVtech-Resources/blob/master/Gyro_Raw%20Noise%20Profiles/Clean/Nova%20on%20BF4.0.png" width="250"></a><br/>
"Better" means the spektrograph lines are lower or have more well defined peaks.  An important factor is the dip in raw motor vibrations ("noise") from 80 to 200hz.

---
#### Brushless Whoop Class (based on Mobula 7 w/ 2s)
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 100

set dyn_lpf_gyro_min_hz = 100
set dyn_lpf_gyro_max_hz = 300
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 0
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 0

set dyn_lpf_dterm_min_hz = 70
set dyn_lpf_dterm_max_hz = 170
set dterm_lowpass_type = BiQUAD
set dterm_lowpass_hz = 0
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 5000

set p_pitch = 80
set i_pitch = 25
set d_pitch = 80
set f_pitch = 100

set p_roll = 80
set i_roll = 25
set d_roll = 80
set f_roll = 100

set p_yaw = 90
set i_yaw = 90
set d_yaw = 0
set f_yaw = 100

set d_min_pitch = 0
set d_min_roll = 0
set d_min_boost_gain = 30
set d_min_advance = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1750
save

```
</details>

---
#### 2" - 3" Quad - 11xx-12xx Motors
```(in coordination with George Hartmann)```
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = BiQUAD
set dyn_notch_min_hz = 150
set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 700
set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 150

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 5000

set p_pitch = 33
set i_pitch = 85
set d_pitch = 35

set p_roll = 28
set i_roll = 78
set d_roll = 32

set d_min_pitch = 18
set d_min_roll = 16
set d_min_boost_gain = 30
set d_min_advance = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
save

```
</details>

---
#### 3" Quad - 14xx-15xx Motors
```(in coordination with George Hartmann)```
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = BiQUAD
set dyn_notch_min_hz = 150
set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 650
set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 150

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 5000

set p_pitch = 38
set i_pitch = 85
set d_pitch = 35

set p_roll = 35
set i_roll = 78
set d_roll = 32

set d_min_pitch = 18
set d_min_roll = 16
set d_min_boost_gain = 30
set d_min_advance = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
save

```
</details>
<br/><br/>

#### 5" Quad - Setpoint Tracker - Lower Cutoffs Filters (more filtering)
For: 650g to 725g AUW Kwads | 1000 to 1100 deg/sec rates. <br/>
Kwads with Noise between 50hz and 200hz
<br/>

<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 80

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_gyro_max_hz = 0
set gyro_lowpass_hz = 0
set gyro_lowpass_type = PT1
set gyro_lowpass2_hz = 200
set gyro_lowpass2_type = PT1


set dyn_lpf_dterm_min_hz = 0
set dyn_lpf_dterm_max_hz = 0
set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 0
set dterm_lowpass2_type = BiQUAD
set dterm_lowpass2_hz = 125

#For RPM Filter: Without RPM leave at = 8 (default)
#Set to 0 if you can afford less Dynamic Notch filtering because RPM is added (reduces to one notch instead of two on DN)
#set dyn_notch_width_percent = 8

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set p_pitch = 60
set i_pitch = 70
set d_pitch = 60
set f_pitch = 350

set p_roll = 65
set i_roll = 60
set d_roll = 65
set f_roll = 325

set p_yaw = 100
set i_yaw = 100
set d_yaw = 0
set f_yaw = 125

set d_min_pitch = 45
set d_min_roll = 45
set d_min_boost_gain = 30
set d_min_advance = 0
set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1750
save

```
</details>

---
#### 5" Quad - Setpoint Tracker - Higher Cutoffs Filters (less filtering)
For: 650g to 725g AUW Kwads | 1000 to 1100 deg/sec rates. <br/>
Kwads with NO Noise between 50hz and 200hz
<br/>
**Requires a quad with NO noise issues from 50hz to 200hz! So, if you are not sure, use the above first and if that goes well try these settings next.**
<br/>
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 80

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_gyro_max_hz = 0
set gyro_lowpass_hz = 0
set gyro_lowpass_type = PT1
set gyro_lowpass2_hz = 0
set gyro_lowpass2_type = PT1

set dyn_lpf_dterm_min_hz = 80
set dyn_lpf_dterm_max_hz = 175
set dterm_lowpass_type = BiQUAD
set dterm_lowpass_hz = 0
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 0

#For RPM Filter: Without RPM leave at = 8 (default)
#Set to 0 if you can afford less Dynamic Notch filtering because RPM is added (reduces to one notch instead of two on DN)
#set dyn_notch_width_percent = 8

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set p_pitch = 60
set i_pitch = 70
set d_pitch = 60
set f_pitch = 350

set p_roll = 65
set i_roll = 60
set d_roll = 65
set f_roll = 325

set p_yaw = 100
set i_yaw = 100
set d_yaw = 0
set f_yaw = 125

set d_min_pitch = 45
set d_min_roll = 45
set d_min_boost_gain = 30
set d_min_advance = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1750
save

```
</details>
<br/><br/>

---
#### 6"/7" Quads
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_min_hz = 100
set dyn_lpf_gyro_max_hz = 300
set dyn_lpf_dterm_min_hz = 100
set dyn_lpf_dterm_max_hz = 200
set dterm_lowpass_type = PT1
set dterm_lowpass2_hz = 0

#For RPM Filter: Without RPM leave at = 8 (default)
#Set to 0 if you can afford less Dynamic Notch filtering because RPM is added (reduces to one notch instead of two on DN)
#set dyn_notch_width_percent = 8

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000

set d_pitch = 58
set d_roll = 55

set d_min_pitch = 28
set d_min_roll = 25
set d_min_boost_gain = 45
set d_min_advance = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
save

```
</details>

---
#### X-Class [IN PROGRESS!!!]
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 50
set dyn_lpf_gyro_min_hz = 50
set dyn_lpf_gyro_max_hz = 200
set dyn_lpf_dterm_min_hz = 60
set dyn_lpf_dterm_max_hz = 150
set dterm_lowpass_type = BIQUAD
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set d_pitch = ??
set d_roll = ??
set d_min_pitch = ??
set d_min_roll = ??
set d_min_boost_gain = 45
set d_min_advance = 0

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1750
save

```
</details>

---
#### PID Tuning Preset
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 80
set gyro_lowpass_type = PT1
set dyn_lpf_gyro_min_hz = 100
set dyn_lpf_gyro_max_hz = 350
set gyro_lowpass2_hz = 0
set dterm_lowpass_type = BiQUAD
set dyn_lpf_dterm_min_hz = 80
set dyn_lpf_dterm_max_hz = 175
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000

set f_pitch = 0
set f_roll = 0
set f_yaw = 0

set d_min_pitch = 0
set d_min_roll = 0

set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#PID Controller Settings
set feedforward_transition = 0
set abs_control_gain = 0
set use_integrated_yaw = OFF
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
save

```
</details>

---

<br/>
<br/>

## Pilot: RipperDrone
#### 5" Quad (5s-6s)
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_max_hz = 510
set dterm_lowpass_type = PT1
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set p_pitch = 35
set d_pitch = 25
set f_pitch = 32
set p_roll = 33
set d_roll = 23
set f_roll = 30
set p_yaw = 30
set i_yaw = 90
set d_min_roll = 0
set d_min_pitch = 0

```
</details>

---

<br/>
<br/>

## Pilot: CUDA KEER<br>
#### [3" toothfairy with Emax RS1306b BF4.2](https://github.com/cudakeer808/betaflight/wiki/3%22-toothfairy-with-Emax-1306-4000kv-BF4.2)
#### [Floss 2.1 hybrid with Hyperlite 2207 1922kv BF4.2 race rig](https://github.com/cudakeer808/betaflight/wiki/floss-2.1-2207-1922-BF4.2)
#### [6" Floss 2.1 with Hyperlite 2207 1922kv BF4.0 race rig](https://github.com/cudakeer808/betaflight/wiki/floss-2.1-2207-1922-rpm_filter)<br>
#### [5" Massive Droner with T-Motor F60pro II](https://github.com/cudakeer808/betaflight/wiki/Massive-Droner-2207-1750kv)
<br/>
<br/>
---


## Pilot: TehLlama <br>
#### 6" Neutron-R with BrotherHobby R6 2207 Race Rig<br>
##### [Build Details](https://rotorbuilds.com/build/18676)

<details><summary>CLI Copy\Paste</summary>

```python
# Filter Settings
set dyn_lpf_dterm_min_hz = 91
set dyn_lpf_dterm_max_hz = 221
set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 150
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 225

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 3000
set p_pitch = 53
set i_pitch = 70
set d_pitch = 37
set f_pitch = 180
set p_roll = 48
set i_roll = 68
set d_roll = 34
set f_roll = 170
set p_yaw = 40
set i_yaw = 80
set d_yaw = 0
set f_yaw = 100
set d_min_pitch = 24
set d_min_roll = 27

# This is my preferred, but unconventional iTermRelax Setup - relies on FF to push Yaw axis around
set iterm_relax = RPY
set iterm_relax_type = SETPOINT
set iterm_relax_cutoff = 12

# Rates - These are good novice racer rates
# Max stick deflection values are 375°/375°/275° rates (R/P/Y)
set rates_type = BETAFLIGHT
set roll_rc_rate = 75
set pitch_rc_rate = 75
set yaw_rc_rate = 55
set roll_expo = 0
set pitch_expo = 0
set yaw_expo = 0
set roll_srate = 60
set pitch_srate = 60
set yaw_srate = 60


```
</details>

---
<br/>

## REVERT 4.0.x DEFAULTS TO AN OLDER VERSION OF BETAFLIGHT DEFAULTS
These are presets to apply to BF 4.0.x to get the defaults of an older versions of Betaflight.  This WILL give you the same flight experience as an older release if that is what you like, but want the new features of BF 4.0.x.
#### Revert to BF 3.5.X
<details><summary>CLI Copy\Paste</summary>

```python
#Features to be off by Default
feature -AIRMODE

# Filter Settings
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 100
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 300

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_dterm_min_hz = 0

set dyn_notch_range = Auto
set dyn_notch_width_percent = 0
set dyn_notch_q = 70
set dyn_notch_min_hz = 130

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 100
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 200

set d_min_roll = 0
set d_min_pitch = 0
set d_min_yaw = 0

# Default PIDs
set p_pitch = 50
set i_pitch = 50
set d_pitch = 27
set f_pitch = 60
set p_roll = 46
set i_roll = 45
set d_roll = 25
set f_roll = 60
set p_yaw = 65
set i_yaw = 45
set d_yaw = 0
set f_yaw = 60

#PID Controller Settings
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 11
set abs_control_gain = 0
set use_integrated_yaw = OFF

set tpa_mode = PD
set tpa_rate = 10
set tpa_breakpoint = 1650

#RC smoothing settings
set rc_smoothing_type = INTERPOLATION
save

```
</details>

---
#### Revert to BF 3.4.X
<details><summary>CLI Copy\Paste</summary>

```python
#Features to be off by Default
feature -AIRMODE

# Filter Settings
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 100
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 300

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_dterm_min_hz = 0

set dyn_notch_range = Low
set dyn_notch_width_percent = 0
set dyn_notch_q = 70
set dyn_notch_min_hz = 125

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 100
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 200

set d_min_roll = 0
set d_min_pitch = 0
set d_min_yaw = 0

# Default PIDs
set p_pitch = 50
set i_pitch = 50
set d_pitch = 27
set f_pitch = 150
set p_roll = 46
set i_roll = 45
set d_roll = 25
set f_roll = 150
set p_yaw = 70
set i_yaw = 45
set d_yaw = 0
set f_yaw = 0

#PID Controller Settings
set feedforward_transition = 0
set abs_control_gain = 0
set use_integrated_yaw = OFF
set anti_gravity_gain = 5000

#I-term Relax is OFF by Default
set iterm_relax_type = Setpoint
set iterm_relax_cutoff = 100
#if you turn on i-Term Relax, 3.4 defaults are below (take "#" off front)
#set iterm_relax_type = Gyro
#set iterm_relax_cutoff = 11

set tpa_mode = PD
set tpa_rate = 10
set tpa_breakpoint = 1650

#RC smoothing settings
set rc_smoothing_type = INTERPOLATION
save

```
</details>

---
#### Revert to BF 3.2.x & 3.3.X (they were the same)
<details><summary>CLI Copy\Paste</summary>

```python
#Features to be off by Default
feature -AIRMODE
feature -DYNAMIC_FILTER
feature -ANTI_GRAVITY

# Filter Settings
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 90
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 0

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_dterm_min_hz = 0

set gyro_notch1_hz = 400
set gyro_notch1_cutoff = 300
set gyro_notch2_hz = 200
set gyro_notch2_cutoff = 100

set dyn_notch_range = Low
set dyn_notch_width_percent = 0
set dyn_notch_q = 70
set dyn_notch_min_hz = 125

set dterm_lowpass_type = BIQUAD
set dterm_lowpass_hz = 100
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 0
set dterm_notch_hz = 260
set dterm_notch_cutoff = 160

set d_min_roll = 0
set d_min_pitch = 0
set d_min_yaw = 0

# Default PIDs
set p_pitch = 58
set i_pitch = 50
set d_pitch = 35
set f_pitch = 0
set p_roll = 40
set i_roll = 40
set d_roll = 30
set f_roll = 0
set p_yaw = 70
set i_yaw = 45
set d_yaw = 0
set f_yaw = 0

#PID Controller Settings
set feedforward_transition = 0
set iterm_relax = OFF
set abs_control_gain = 0
set use_integrated_yaw = OFF
set anti_gravity_gain = 1000

set tpa_mode = PD
set tpa_rate = 10
set tpa_breakpoint = 1650

#RC smoothing settings
set rc_smoothing_type = INTERPOLATION
save

```



