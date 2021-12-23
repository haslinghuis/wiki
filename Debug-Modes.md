### INFORMATION COMMANDS
* "VERSION" - Shows the current firmware loaded and the last github code commit [ie: "(9f67a584b)"] for the specific build of the firmware.

* "STATUS" - Shows various information about the quad setup such as ROM space available for firmware, gyro type, detected voltage, etc...

* "TASKS" - Shows the active tasks running and their CPU utilization.  Make sure the "gyro/PID tasks rate/hz is running at the specified rate.

* "DSHOT_TELEMETRY_INFO" - Shows the Dshot RPM bi-directional telemetry packet success for each ESC. (4.1+)

* "RC_SMOOTHING_INFO" - Shows the detected RX frame rate.  This only works if the "Filter" RC signal smoothing type is selected in the Receiver tab, and both "Input Cutoff Type" and "Derivative Cutoff Type" are set to "auto".  The radio and RX need to be connected and powered up for the detected frame rate data to be valid.

DSHOT_RPM_ERRORS:
* [0] = Motor #1: the per-motor invalid packet percentages in hundredths of a percent (so 123 is 1.23%)
* [1] = Motor #2:   "
* [2] = Motor #3:   "
* [3] = Motor #4:   "

SMARTAUDIO:
* [0] = SmartAudio Version * 100 + Device Mode
* [1] = Device Channel
* [2] = Device Frequency
* [3] = Device Power

DYN_IDLE:

* [0] = motorRangeMinIncrease * 1000
* [1] = targetRpsChangeRate (simple RPM error * idle_adjustment_speed)
* [2] = error (amount of error to fix)
* [3] = minRps (lowest current motor rpm, in revolutions per second * 10)

So, for example, a minRps value of 500 corresponds to 50.0 * 60 = 3000rpm

CYCLETIME:
* [0] = The time in microseconds since the PID task last ran
* [1] = The current CPU load in percent
* [2] = The time since the previous motor update (uS)
* [3] = The variance in the motor update interval vs. the target PID loop time (uS)

note: with 4.2.x restructuring of the gyro/PID loops [2] & [3] are pointless since the motor updates are part of the PID task now.

PIDLOOP:
* [0] The time the Gyro Task ran (Useless post BF 4.2.x)
* [1] Time microseconds the PID calculations
* [2] Time in microseconds for mixer, servos, motor update, dshot telemetry stats
* [3] Time in microseconds the mag hold and blackbox processing logic

DEBUG_SCHEDULER_DETERMINISM: Requires USE_LATE_TASK_STATISTICS to be defined so not present on F411
* [0] - Gyro task start cycle time in 10th of a us
* [1] - ID of late task
* [2] - Amount task is late in 10th of a us
* [3] - Gyro lock skew in clock cycles

DEBUG_TIMING_ACCURACY: Requires USE_LATE_TASK_STATISTICS to be defined so not present on F411
* [0] - % CPU busy
* [1] - Tasks late in last second
* [2] - Total lateness in last second in 10ths us
* [3] - Total tasks run in last second

### GYRO SIGNAL (https://youtu.be/A09sprstYqI)
GYRO_RAW: (does have gyro's internal Digital Lowpass Filter (DLPF) applied)

For use in seeing the unscaled gyro signal into the firmware for use in stack overflow detection (ICM gyros).
* [0] = roll: gyro signal input to firmware **UN**scaled
* [1] = pitch: gyro signal input to firmware **UN**scaled
* [2] = yaw: gyro signal input to firmware **UN**scaled
* [3] = [empty]

GYRO_SCALED: (does have gyro's internal Digital Lowpass Filter (DLPF) applied)

For use in seeing the "raw" noise from the gyro into the firmware for filter tuning.
* [0] = roll: gyro signal input to firmware **scaled** into deg/sec
* [1] = pitch: gyro signal input to firmware **scaled** into deg/sec
* [2] = yaw: gyro signal input to firmware **scaled** into deg/sec
* [3] = [empty]

GYRO_FILTERED: (SAME AS GYRO TRACES RECORDED BY DEFAULT)
* [0] = roll: filtered gyro trace
* [1] = pitch: filtered gyro trace
* [2] = yaw: filtered gyro trace
* [3] = [empty]


### DYNAMIC NOTCH AND LOWPASS FILTERS (https://youtu.be/__vyp60cU_8)
D_LPF:
* [0] = roll: unfiltered D
* [1] = pitch: unfiltered D
* [2] = roll: filtered, pre application of TPA
* [3] = pitch: filtered, pre application of TPA

DYN_LPF:
* [0] = roll: raw gyro data (scaled)
* [1] = roll: notch center frequency
* [2] = roll: lowpass filter cutoff frequency
* [3] = roll: pre-dyn notch (post lowpass filters)gyro data

FFT_FREQ:
* [0] = gyroDebugAxis: notch 1 center frequency
* [1] = gyroDebugAxis: notch 2 center frequency
* [2] = gyroDebugAxis: notch 3 center frequency
* [3] = gyroDebugAxis: pre-dyn notch gyro data (post lowpass and RPM filter)

FFT:
* [0] = gyroDebugAxis: pre-dyn notch gyro data (post lowpass and RPM filter)
* [1] = gyroDebugAxis: post-dyn notch gyro data
* [2] = gyroDebugAxis: downsampled data used for FFT
* [3] = [empty]

FFT_TIME:
* [0] = currently active calculation step
* [1] = duration of this step
* [2] = [empty]
* [3] = [empty]

RPM_FILTER:
* [0] = motor #1 RPM Notch center frequency (where peak motor noise is anticipated)
* [1] = motor #2 RPM Notch center frequency
* [2] = motor #3 RPM Notch center frequency
* [3] = motor #4 RPM Notch center frequency

DSHOT_RPM_TELEMETRY: (eRPM x # motor poles = RPM)
* [0] = motor #1 eRPM
* [1] = motor #2 eRPM
* [2] = motor #3 eRPM
* [3] = motor #4 eRPM

### RC COMMAND SMOOTHING (https://youtu.be/M50fKpvFjT8)

RC_INTERPOLATION:
* [0] = raw un-smoothed rc channel data [roll]
* [1] = current RX frame rate
* [2] = interpolation step count
* [3] = rc setpoint [roll]

RC_SMOOTHING:
* [0] = raw un-smoothed rc channel data
* [1] = raw un-smoothed setpoint derivative
* [2] = filtered setpoint derivative before applied to setpoint weight
* [3] = the current calculated average (shows the current "locked" rate used to set the filters)

RC_SMOOTHING_RATE:
* [0] = log each RX frame interval (shows the delay from the previous frame in microsecond)
* [1] = log the training step count
* [2] = the current calculated average (shows the current "locked" rate used to set the filters)
* [3] = indicates whether guard time is active

### FLIGHT DYNAMICS
AC_ERROR (Absolute Control Error):
* [0] = roll: axis error * 10
* [1] = pitch: axis error * 10
* [2] = yaw: axis error * 10
* [3] = [none]

AC_CORRECTION (AC = Absolute Control):
* [0] = roll: axis AC correction * 10
* [1] = pitch: axis AC correction * 10
* [2] = yaw: axis AC correction * 10
* [3] = [none]

FF_THUMB (Absolute Control Correction):
* [0] = roll normal FF
* [1] = roll FF after stick limit
* [2] = FF after max deflection
* [3] = projected max rate due to stick extrapolation

ITERM_RELAX: (https://youtu.be/QfiGTG5LfCk)
* [0] = highpass filter to detect large setpoint changes
* [1] = relax factor (percent, only used in `SETPOINT` mode)
* [2] = relaxed I-term Error
* [3] = absolute control axis error [roll]

D_MIN:
* [0] = gyro factor (percent, scaled by `d_min_gain`)
* [1] = setpoint factor (percent, scaled by `d_min_advance`). The larger of _gyro_ and _setpoint_ factors takes effect.
* [2] = roll: active D-term gain
* [3] = pitch: active D-term gain

### SENSOR FUSION GYRO BOARDS:
DUAL_GYRO_RAW:
* [0] = roll: RAW gyro #1 data (NOT scaled to Deg/sec)
* [1] = pitch: RAW gyro #1 data (NOT scaled to Deg/sec)
* [2] = roll: RAW gyro #2 data (NOT scaled to Deg/sec)
* [3] = pitch: RAW gyro #2 data (NOT scaled to Deg/sec)

DUAL_GYRO_SCALED:
* [0] = roll: RAW SCALED gyro #1 data (scaled to Deg/sec)
* [1] = pitch: RAW SCALED gyro #1 data (scaled to Deg/sec)
* [2] = roll: RAW SCALED gyro #2 data (scaled to Deg/sec)
* [3] = pitch: RAW SCALED gyro #2 data (scaled to Deg/sec)

DUAL_GYRO_DIFF:
* [0] = roll: gyro #1 filter – gyro #2 filtered
* [1] = pitch: gyro #1 filter – gyro #2 filtered
* [2] = yaw: gyro #1 filter – gyro #2 filtered
* [3] = [empty]

DUAL_GYRO_COMBINED:  (programmer useful only)
* [0] = [empty]
* [1] = roll: filtered gyro (same as “gyro” trace)
* [2] = pitch: filtered gyro (same as “gyro” trace)
* [3] = [empty]

DUAL_GYRO_COMBINED:  (programmer useful only)
* [0] = [empty]
* [1] = roll: filtered gyro (same as “gyro” trace)
* [2] = pitch: filtered gyro (same as “gyro” trace)
* [3] = [empty]

### ExpressLRS SPI Receiver 

DEBUG_RX_EXPRESSLRS_PHASELOCK (ExpressLRS software based PPL)
* [0] = rawOffsetUs: instantaneous phase offset measured by last timer tick 
* [1] = offsetUs: filtered offset value used in software PLL
* [2] = frequencyOffsetTicks: frequency offset (in timer ticks) between ELRS transmitter and RX
* [3] = phaseShiftUs: current instantaneous phase shift value that will applied next timer tick

DEBUG_RX_EXPRESSLRS_SPI (ExpressLRS SPI RX)
* [0] = lostConnectionCounter: counts the number of times the connection has been lost since startup
* [1] = rssiFiltered: current low-pass filtered RSSI value reported from sx1280/sx127x
* [2] = snr: current SNR reported by sx1280/sx127s
* [3] = uplinkLQ: uplink link quality percentage 

### Debug 

| DEBUG TYPE |
| :--- |
| DEBUG_CYCLETIME |
| DEBUG_BATTERY |
| DEBUG_GYRO_FILTERED |
| DEBUG_ACCELEROMETER |
| DEBUG_PIDLOOP |
| DEBUG_GYRO_SCALED |
| DEBUG_RC_INTERPOLATION |
| DEBUG_ANGLERATE |
| DEBUG_ESC_SENSOR |
| DEBUG_SCHEDULER |
| DEBUG_STACK |
| DEBUG_ESC_SENSOR_RPM |
| DEBUG_ESC_SENSOR_TMP |
| DEBUG_ALTITUDE |
| DEBUG_FFT |
| DEBUG_FFT_TIME |
| DEBUG_FFT_FREQ |
| DEBUG_RX_FRSKY_SPI |
| DEBUG_RX_SFHSS_SPI |
| DEBUG_GYRO_RAW |
| DEBUG_DUAL_GYRO_RAW |
| DEBUG_DUAL_GYRO_DIFF |
| DEBUG_MAX7456_SIGNAL |
| DEBUG_MAX7456_SPICLOCK |
| DEBUG_SBUS |
| DEBUG_FPORT |
| DEBUG_RANGEFINDER |
| DEBUG_RANGEFINDER_QUALITY |
| DEBUG_LIDAR_TF |
| DEBUG_ADC_INTERNAL |
| DEBUG_RUNAWAY_TAKEOFF |
| DEBUG_SDIO |
| DEBUG_CURRENT_SENSOR |
| DEBUG_USB |
| DEBUG_SMARTAUDIO |
| DEBUG_RTH |
| DEBUG_ITERM_RELAX |
| DEBUG_ACRO_TRAINER |
| DEBUG_RC_SMOOTHING |
| DEBUG_RX_SIGNAL_LOSS |
| DEBUG_RC_SMOOTHING_RATE |
| DEBUG_ANTI_GRAVITY |
| DEBUG_DYN_LPF |
| DEBUG_RX_SPEKTRUM_SPI |
| DEBUG_DSHOT_RPM_TELEMETRY |
| DEBUG_RPM_FILTER |
| DEBUG_D_MIN |
| DEBUG_AC_CORRECTION |
| DEBUG_AC_ERROR |
| DEBUG_DUAL_GYRO_SCALED |
| DEBUG_DSHOT_RPM_ERRORS |
| DEBUG_CRSF_LINK_STATISTICS_UPLINK |
| DEBUG_CRSF_LINK_STATISTICS_PWR |
| DEBUG_CRSF_LINK_STATISTICS_DOWN |
| DEBUG_BARO |
| DEBUG_GPS_RESCUE_THROTTLE_PID |
| DEBUG_DYN_IDLE |
| DEBUG_FF_LIMIT |
| DEBUG_FF_INTERPOLATED |
| DEBUG_BLACKBOX_OUTPUT |
| DEBUG_GYRO_SAMPLE |
| DEBUG_RX_TIMING |
| DEBUG_D_LPF |
| DEBUG_VTX_TRAMP |
| DEBUG_GHST |
| DEBUG_RX_EXPRESSLRS_SPI |
| DEBUG_RX_EXPRESSLRS_PHASELOCK |
