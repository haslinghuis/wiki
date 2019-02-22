# DYNAMIC NOTCH AND LOWPASS FILTERS
DYN_LPF:
[0] = roll: raw gyro data
[1] = roll: notch center frequency
[2] = roll: lowpass filter cutoff frequency
[3] = roll: pre-dyn notch (post lowpass filters)gyro data

FFT_FREQ:
[0] = roll: notch center frequency
[1] = pitch: notch center frequency
[2] = roll: raw gyro data
[3] = roll: pre-dyn notch (post lowpass filters) gyro data

FFT:
[0] = pre-dyn notch gyro roll data
[1] = gyro roll data only filtered with the dynamic notch
[2] = resampled and bandpass filtered data used for FFT
[3] = weighted center Index of the FFT-bins * 100. This is used to calculate the center frequency

FFT_TIME:
[0] = currently active calculation step
[1] = duration of this step
[2] = duration of additional steps
