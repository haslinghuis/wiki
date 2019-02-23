### GYRO SIGNAL (https://youtu.be/A09sprstYqI)
GYRO_RAW: (does have gyro's internal Digial Lowpass Filter (DLPF) applied)

For use in seeing the unscaled gyro signal into the firmware for use in stack overflow detection (ICM gyros).
* [0] = roll: gyro signal input to firmware **UN**scaled
* [1] = pitch: gyro signal input to firmware **UN**scaled
* [2] = yaw: gyro signal input to firmware **UN**scaled
* [3] = [empty]

GYRO_SCALED: (does have gyro's internal Digial Lowpass Filter (DLPF) applied)

For use in seeing the "raw" noise from the gyro into the firmware for fitler tunining.
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
DYN_LPF:
* [0] = roll: raw gyro data (scaled)
* [1] = roll: notch center frequency
* [2] = roll: lowpass filter cutoff frequency
* [3] = roll: pre-dyn notch (post lowpass filters)gyro data

FFT_FREQ:
* [0] = roll: notch center frequency
* [1] = pitch: notch center frequency
* [2] = roll: raw gyro data (scaled)
* [3] = roll: pre-dyn notch (post lowpass filters) gyro data

FFT:
* [0] = pre-dyn notch gyro roll data
* [1] = gyro roll data only filtered with the dynamic notch
* [2] = resampled and bandpass filtered data used for FFT
* [3] = weighted center Index of the FFT-bins * 100. This is used to calculate the center frequency

FFT_TIME:
* [0] = currently active calculation step
* [1] = duration of this step
* [2] = duration of additional steps
* [3] = [empty]

RPM_FILTER:
* [0] = motor #1 RPM Notch center frequency (where peak motor noise is anticipated)
* [1] = motor #2 RPM Notch center frequency
* [2] = motor #3 RPM Notch center frequency
* [3] = motor #4 RPM Notch center frequency

DSHOT_RPM_TELEMETRY:
* [0] = motor #1 RPM
* [1] = motor #2 RPM
* [2] = motor #3 RPM
* [3] = motor #4 RPM

### RC COMMAND SMOOTHING (https://youtu.be/M50fKpvFjT8)

RC_INTERPOLATION:
* [0] = raw un-smoothed rc channel data
* [1] = current RX frame rate
* [2] = [empty]
* [3] = [empty]

RC_SMOOTHING:
* [0] = raw un-smoothed rc channel data
* [1] = raw un-smoothed setpoint derivative
* [2] = filtered setpoint derivative before applied to setpoint weight
* [3] = auto-calculated filter cutoff frequency base after sampling the rx frame rate

RC_SMOOTHING_RATE:
* [0] = log each RX frame interval
* [1] = log the training step count
* [2] = the current calculated average
* [3] = indicates whether guard time is active

### FLIGHT DYNAMICS
ITERM_RELAX: (https://youtu.be/QfiGTG5LfCk)
* [0] = highpass filter to detect large setpoint changes
* [1] = ?? (mirror of highpass?)
* [2] = accumulation of the I-term
* [3] = output I-term (same as I-term trace)

D_MIN:
* [0] = roll: active D-term gains
* [1] = pitch: active D-term gains
* [2] = yaw: active D-term gains
* [3] = [empty]

### SENSOR FUSION GYRO BOARDS:
DUAL_GYRO_RAW:
* [0] = roll: RAW gyro #1 data
* [1] = pitch: RAW gyro #1 data
* [2] = roll: RAW gyro #2 data
* [3] = pitch: RAW gyro #2 data

DUAL_GYRO:
* [0] = roll: FILTERED gyro #1 data
* [1] = pitch: FILTERED gyro #1 data
* [2] = roll: FILTERED gyro #2 data
* [3] = pitch: FILTERED gyro #2 data

DAUL_GYRO_DIFF:
* [0] = roll: gyro #1 filter – gyro #2 filtered
* [1] = pitch: gyro #1 filter – gyro #2 filtered
* [2] = yaw: gyro #1 filter – gyro #2 filtered
* [3] = [empty]

DUAL_GYRO_COMBINED:  (programmer useful only)
* [0] = [empty]
* [1] = roll: filtered gyro (same as “gyro” trace)
* [2] = pitch: filtered gyro (same as “gyro” trace)
* [3] = [empty]


