# BF 4.0.X

## UAV TECH
### Brushed Whoop Class

### 2" - 3" Quad

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
set pidsum_limit_yaw = 1000
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
set dyn_lpf_dterm_min_hz = 100
set dyn_lpf_dterm_max_hz = 200
set dterm_lowpass_type = PT1
set dterm_lowpass2_hz = 0

#PID Gains Settings
set vbat_pid_gain = ON
set anti_gravity_gain = 10000
set iterm_relax_cutoff = 11
set pidsum_limit_yaw = 1000
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

### Revert to BF 3.5.X

### Revert to BF 3.4.X

### Revert to BF 3.3.X

### Revert to BF 3.2.X