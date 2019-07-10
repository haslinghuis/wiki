### The below presets are _**by the community, for the community**_.  You will see the BF version the preset is targeted toward and the pilot's name who published their recommended preset(s).  We encourage community members to provide their own presets on this wiki page.  To do so, you simply need a Github account.  Enjoy!

<p> To use, simply copy and paste the preset CLI commands into the Betaflight CLI tab.  After the paste, type "save" and hit [enter].  That will load the settings of the preset which you can see (mostly) through the GUI tabs.</p>

<br/><br/>

# BF 4.0.X

<br/>

## Pilot: UAV TECH

<b>
**ALWAYS CHECK YOUR MOTOR TEMPS AFTER A SHORT 15 SECOND FORWARD FLIGHT** <br/>
If your mechanical/electrical issues are not addressed (most of the time those are the issues folks have), the below settings may require heavier filtering and lower PID gains (the BF default).  Heavier filtering and lower PID gains can provide for cooler motors, but also has worse flight performance.<br/><br/>
</b>

A clean build is when a spectrograph of the RAW gyro noise trace (Debug_Mode=Gyro_Scaled) on a FULL & HARD flight looks like the below or better: https://github.com/spatzengr/UAVtech-Resources/blob/master/Gyro_Raw%20Noise%20Profiles/Clean/Nova%20on%20BF4.0.png

---
#### Brushed \ Brushless Whoop Class
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 150
set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 750
set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 0

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

```
</details>

---
#### 5" Quad on Clean Build
<details><summary>CLI Copy\Paste</summary>

```python
#Filter Settings
set dyn_notch_min_hz = 80
set gyro_lowpass_type = BiQUAD
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_gyro_max_hz = 0
set dyn_lpf_dterm_min_hz = 80
set dyn_lpf_dterm_max_hz = 175
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 0

#For RPM Filter: Without RPM leave at = 8 (default)
#Set to 0 if you can afford less Dynamic Notch filtering because RPM is added (reduces to one notch instead of two on DN)
#set dyn_notch_width_percent = 8

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set p_pitch = 60
set i_pitch = 70
set d_pitch = 55

set p_roll = 65
set i_roll = 60
set d_roll = 55

set d_min_pitch = 45
set d_min_roll = 45
set d_min_boost_gain = 45
set d_min_advance = 0
set pidsum_limit = 1000 #unleashes PID Sum to be 100% (not restricted to 50% by default)

#Assumes Freestyle | For racing use "Setpoint" and cutoff = 20
set iterm_relax_type = GYRO
set iterm_relax_cutoff = 10

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800

```
</details>

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
set iterm_relax_cutoff = 11

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800

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
#### [6" Floss 2.1 with Hyperlite 2405 1922kv race rig](https://github.com/cudakeer808/betaflight/wiki/floss-2.1-2207-1922-rpm_filter)

---

<br/>
<br/>
<br/>

## REVERT TO AN OLDER VERSION OF BETAFLIGHT
These are presets to apply to BF 4.0 to get the defaults from older versions of betaflight
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

```
</details>
