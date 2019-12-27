### The below presets are _**by the community, for the community**_.  You will see the BF version the preset is targeted toward and the pilot's name who published their recommended preset(s).  We encourage community members to provide their own presets on this wiki page.  To do so, you simply need a Github account.  Enjoy!

### IMPORTANT: These settings are NOT provided or endorsed by the Betaflight project. They are examples that others have found helpful for their particular quad. THEY MAY NOT BE SUITABLE FOR YOUR QUAD! A better use might be to examine similar configurations to yours and get ideas on possible tuning directions rather than blindly copy/pasting someone else's settings. Always test carefully and safely.

<p> To use, simply copy and paste the preset CLI commands into the Betaflight CLI tab.  After the paste, type "save" and hit [enter].  That will load the settings of the preset which you can see (mostly) through the GUI tabs.</p>

<br/><br/>

# BF 4.1.X
Betaflight 4.1 has the tuning sliders, someone should fill this out.

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
#### [6" Floss 2.1 with Hyperlite 2207 1922kv race rig](https://github.com/cudakeer808/betaflight/wiki/floss-2.1-2207-1922-rpm_filter)<br>
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



