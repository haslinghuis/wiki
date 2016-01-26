Im sure this needs to be revised but this is what I started a couple weeks ago in te thread.

'''set rc_smoothing''' = OFF 
Interpolation of Rc data during looptimes when there are no new updates. This gives smoother RC input to PID controller and cleaner PIDsum
 
'''set roll_yaw_cam_mix_degrees''' = 0
Need feedback about proof of concept feature for mixing of roll and yaw to FPV cam angle. set roll_yaw_cam_mix_degrees = <degrees>
 
'''set enable_fast_pwm''' = OFF
 
'''set gyro_lpf''' = 188HZ ''Must be set to OFF to enable 2K mode''
 F1 boards must also set acc_hardware = 1 Disabling any level modes
Hardware lowpass filter for gyro. Allowed values depend on the driver - For example MPU6050 allows 5,10,20,42,98,188,256Hz, while MPU3050 doesn't allow 5Hz. If you have to set gyro lpf below 42Hz generally means the frame is vibrating too much, and that should be fixed first. Values outside of supported range will usually be ignored by drivers, and will configure lpf to default value of 42Hz.

'''set acc_hardware''' = 0 (default on) (must be set to 1 for 2K mode)
sets the accelerometer 0 means auto detect, 1 means off, 2 for ADXL345, 3 for MPU6050 integrated accelerometer, 4 for MMA8452, 5 for BMA280, 6 for LSM303DLHC, 7 for MPU6000, 8 for MPU6500

'''set baro_hardware''' = 0 (default on) 

'''set mag_hardware''' = 0 (default on)
 
'''set imu_dcm_kp''' = 2500
 
'''set imu_dcm_ki''' = 0
 
'''set beeper_off_flags''' = 256

beeper_off_flags = sum of each desired beeper turned off
 
 +BEEPER_GYRO_CALIBRATED, 1

 +BEEPER_RX_LOST_LANDING, 2 // Beeps SOS when armed and TX is turned off or signal lost (autolanding/autodisarm)

 +BEEPER_RX_LOST, 4 // Beeps when TX is turned off or signal lost (repeat until TX is okay)

 +BEEPER_DISARMING, 8 // Beep when disarming the board

 +BEEPER_ARMING, 16 // Beep when arming the board

 +BEEPER_ARMING_GPS_FIX, 32 // Beep a special tone when arming the board and GPS has fix

 +BEEPER_BAT_CRIT_LOW, 64 // Longer warning beeps when battery is critically low (repeats)

 +BEEPER_BAT_LOW, 128 // Warning beeps when battery is getting low (repeats)

 +BEEPER_, 256 // when plugged into USB

 +BEEPER_RX_SET, 512 // Beeps when aux channel is set for beep or beep sequence how many satellites has found if GPS enabled

 +BEEPER_DISARM_REPEAT, 1024 // Beeps sounded while stick held in disarm position

 +BEEPER_ACC_CALIBRATION, 2048 // ACC inflight calibration completed confirmation

 +BEEPER_ACC_CALIBRATION_FAIL, 4096 // ACC inflight calibration failed

 +BEEPER_READY_BEEP, 8192 // Ring a tone when GPS is locked and ready

 +BEEPER_MULTI_BEEPS, 16384 // Internal value used by 'beeperConfirmationBeeps()'.

 +BEEPER_ARMED, 32768 // Warning beeps when board is armed (repeats until board is disarmed or throttle is increased)
 
'''set acc_cut_hz''' = 15
 
'''set soft_gyro_lpf_hz''' = 60
 
'''set dterm_lpf_hz''' = 0
 
'''set acro_plus_factor''' = 0
 