# BF 4.0.X

## UAV TECH

`**ALWAYS CHECK YOUR MOTOR TEMPS AFTER A SHORT 15 SECOND FORWARD FLIGHT**`

### Brushed Whoop Class

<coming soon>

### 2" - 3" Quad
```
#Filter Settings
set gyro_lowpass_type = PT1
set dyn_notch_min_hz = 100
set dyn_lpf_gyro_min_hz = 150
set dyn_lpf_gyro_max_hz = 750
set dyn_lpf_dterm_min_hz = 150
set dyn_lpf_dterm_max_hz = 350
set dterm_lowpass_type = BiQUAD
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set iterm_relax_cutoff = 11
set d_pitch = 40
set d_roll = 45
set d_min_roll = 20
set d_min_pitch = 25
set d_min_boost_gain = 40
set d_min_advance = 0

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
set iterm_relax_cutoff = 11
set d_pitch = 58
set d_roll = 55
set d_min_roll = 35
set d_min_pitch = 38
set d_min_boost_gain = 45
set d_min_advance = 0

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
set iterm_relax_cutoff = 11
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

### Revert to BF 3.5.X
```
# NB - use your old 'known good' 3.5 PID values, not 4.0 defaults.
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 100
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 300

set dyn_lpf_gyro_min_hz = 0
set dyn_lpf_dterm_min_hz = 0

set dyn_notch_range = AUTO
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

set abs_control_gain = 0
set use_integrated_yaw = OFF
set d_yaw = 0

# If you want to use 4.0 default PIDS, make the following changes:
 set d_pitch = 25
 set d_roll = 23
```
### Revert to BF 3.4.X

<coming soon>

### Revert to BF 3.3.X

<coming soon>

### Revert to BF 3.2.X

<coming soon>