# BF 4.0.X

## UAV TECH

`**ALWAYS CHECK YOUR MOTOR TEMPS AFTER A SHORT 15 SECOND FORWARD FLIGHT**`

### Brushed Whoop Class
```
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 150
set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 750
set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 150

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 5
set p_pitch = 33
set i_pitch = 85
set d_pitch = 35
set p_roll = 28
set i_roll = 78
set d_roll = 32
set d_min_roll = 16
set d_min_pitch = 18
set d_min_boost_gain = 30
set d_min_advance = 0

#Assumes Freestyle | use cutoff = 20 for Racing
set iterm_relax_cutoff = 11

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
```

### 2" - 3" Quad - 11xx-12xx Motors
```
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
set anti_gravity_gain = 5
set p_pitch = 33
set i_pitch = 85
set d_pitch = 35
set p_roll = 28
set i_roll = 78
set d_roll = 32
set d_min_roll = 16
set d_min_pitch = 18
set d_min_boost_gain = 30
set d_min_advance = 0

#Assumes Freestyle | use cutoff = 20 for Racing
set iterm_relax_cutoff = 11

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
```
### 3" Quad - 14xx-15xx Motors
```
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
set anti_gravity_gain = 5
set p_pitch = 38
set i_pitch = 85
set d_pitch = 35
set p_roll = 35
set i_roll = 78
set d_roll = 32
set d_min_roll = 16
set d_min_pitch = 18
set d_min_boost_gain = 30
set d_min_advance = 0

#Assumes Freestyle | use cutoff = 20 for Racing
set iterm_relax_cutoff = 11

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
```
### 5" Quad
Clean build:

Gyro_Scaled spectrograms for a full hard flight look like the below or better:
"Clean Build" (which isn't that crazy clean): https://github.com/spatzengr/UAVtech-Resources/blob/master/Gyro_Raw%20Noise%20Profiles/Clean/Nova%20on%20BF4.0.png
```
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_max_hz = 510
set dterm_lowpass_type = PT1
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set d_pitch = 58
set d_roll = 55
set d_min_roll = 35
set d_min_pitch = 38
set d_min_boost_gain = 45
set d_min_advance = 0

#Assumes Freestyle | use cutoff = 20 for Racing
set iterm_relax_cutoff = 11

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
```
### 6" Quad
```
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_min_hz = 100
set dyn_lpf_gyro_max_hz = 300
set dyn_lpf_dterm_min_hz = 100
set dyn_lpf_dterm_max_hz = 200
set dterm_lowpass_type = PT1
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set iterm_relax_cutoff = 11 #for freestyle | use 20 for racing
set d_pitch = 68
set d_roll = 65
set d_min_roll = 35
set d_min_pitch = 38
set d_min_boost_gain = 45
set d_min_advance = 0

#TPA Settings (which is D-term only by default)
set tpa_rate = 80
set tpa_breakpoint = 1800
```


## REVERT TO AN OLDER VERSION OF BETAFLIGHT

### Revert to BF 3.5.X
```
# Use your old 'known good' 3.5 PID values, not 4.0 defaults.
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

set iterm_relax_type = GYRO
set iterm_relax_cutoff = 11
set abs_control_gain = 0
set use_integrated_yaw = OFF

set tpa_mode = PD
set tpa_rate = 10
set tpa_breakpoint = 1650

```
### Revert to BF 3.4.X

--coming soon--

### Revert to BF 3.3.X

--coming soon--

### Revert to BF 3.2.X

--coming soon--