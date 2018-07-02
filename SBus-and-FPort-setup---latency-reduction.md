# SBus and FPort - Set RF channels to 8 or less

SBus and FPort, in D16 mode, send data packets every 9ms, but the maximum number of channels per packet is 8.

If you only need 8 data channels - four control channels (roll, pitch, yaw and throttle) and four aux switches, **be sure to set the number of channels in the Model Setup page to 8**.  

This will ensure each channel of data is sent every 9ms and ensure proper RC smoothing performance.

If set to more than 8 channels, some channels will only get sent every second packet, i.e. will have a latency of 18ms.  This delay is noticeable and has a negative impact on flight control.  It will also cause unwanted ripples in the motor signals when sticks are moved.

The reason is that derivative or feed-forward components in the PIDs, such as D setpoint weight or throttle boost, require smooth changes in the RC signal.  Without any smoothing, the signal going to the motors will be sharp and spiky.  Lots of stick movement will then then make motors unnecessarily hot and waste battery power, without achieving anything useful.  By appropriately smoothing the RC setpoint changes, these spikes can be rounded off, so the motors sound smoother and don't get so hot.

To provide optimal smoothing, the flight controller needs to know the time interval between new data elements.  

Betaflight assumes that SBus and FPort users will have configured their radio link for 9ms data intervals, as above, by limiting the number of channels in D16 mode to only 8.

In Betaflight 3.4, filter based smoothing sets the optimal filter values automatically if the frequency is set to 0.  For SBus and FPort, Betaflight assumes the user has limited the D16 mode channel count to 8 or less, so that the data interval is 9ms.  On that basis it will typically set the filter frequency to 50Hz.  

If you want to use more than 8 SBus or FPort channels in D16 mode, set the number of channels to 16 so that all will have the same 18ms interval, and manually configure both smoothing filters to 25Hz.  