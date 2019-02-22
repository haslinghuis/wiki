### GYRO SIGNAL
GYRO_RAW: (does have gyro's internal Digial Lowpass Filter (DLPF) applied)
* [0] = roll: gyro signal input to firmware **UN**scaled
* [1] = pitch: gyro signal input to firmware **UN**scaled
* [2] = yaw: gyro signal input to firmware **UN**scaled
* [3] = [empty]

GYRO_SCALED: (does have gyro's internal Digial Lowpass Filter (DLPF) applied)
* [0] = roll: gyro signal input to firmware **scaled** into deg/sec
* [1] = pitch: gyro signal input to firmware **scaled** into deg/sec
* [2] = yaw: gyro signal input to firmware **scaled** into deg/sec
* [3] = [empty]

GYRO_FILTERED: (SAME AS GYRO TRACES RECORDED BY DEFAULT)
* [0] = roll: filtered gyro trace
* [1] = pitch: filtered gyro trace
* [2] = yaw: filtered gyro trace
* [3] = [empty]


### DYNAMIC NOTCH AND LOWPASS FILTERS
DYN_LPF:
* [0] = roll: raw gyro data
* [1] = roll: notch center frequency
* [2] = roll: lowpass filter cutoff frequency
* [3] = roll: pre-dyn notch (post lowpass filters)gyro data

FFT_FREQ:
* [0] = roll: notch center frequency
* [1] = pitch: notch center frequency
* [2] = roll: raw gyro data
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
* [0] = <empty>
* [1] = roll: filtered gyro (same as “gyro” trace)
* [2] = pitch: filtered gyro (same as “gyro” trace)
* [3] = [empty]


