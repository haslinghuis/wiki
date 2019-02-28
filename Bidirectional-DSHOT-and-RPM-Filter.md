### FAQ

- When will bidirectional DSHOT come to F7? Or to F4 FCs with n channels or incompatibilities without burst?

We're currently investigating an alternative implementation approach using GPIO DMA which would likely work on the vast majority of FCs and would then be the one to port to F7.

### Update: first F7 binaries are available at https://github.com/joelucid/rpmfilterf7

| Target | Changes required | Notes | Supported Motors|
| --- | --- | --- | --- |
| ALIENFLIGHTNGF7 | + | M3 doesn't work, use one of M5-9 instead. LED doesn't work with M1 | M1-M2, M4-M9 |
|ALIENWHOOP|-|| M1-M4 | 
|ANYFCF7|-||M1 M2 M3 M4 M5 M6 M9|
|ANYFCM7|+||M1 M2 M3 M4 M5 M7 M9 M10|
|CLRACINGF7|-|Motor 4 doesn't work. Use the LED pad instead|M1 M2 M3 M5|
|DALRCF722DUAL|-||M1-M6. But either M5 or M6|
|EXF722DUAL|+||M1-M8|
|FLYWOOF7DUAL|-||M1-M6|
|FOXEERF722DUAL|+||M1-M6|
|FURYF7|-||M1-M4|
|HAKRCF722|+||M1-M6|
|KAKUTEF7|-||M1-M6|
|KISSFCV2F7|+||M1-M6|
|MATEKF722|+||M1-M8|
|MATEKF722SE|+|M5 does not work|M1-M4, M6-M8|
|NERO|+||M1-M8|
|NUCLEOF7|-|M4 does not work but can be replaced with M6|M1-M3,M6|
|NUCLEOF722|-|M4 does not work but can be replaced with M6|M1-M3,M6|
|OMNIBUSF7|+||M1-M4|
|OMNINXTF7|-||M1-M4|
|SPRACINGF7DUAL|+||M1-M10|
|YUPIF7|-||M1-M6|


### DSHOT & Betaflight 4.0

[Bidirectional DSHOT](https://github.com/betaflight/betaflight/pull/7264) is a new feature in Betaflight 4.0 which enables the flight controller to receive high frequency RPM telemetry for each motor on the ESC motor signal line. It does not require any additional wiring or an additional telemetry back-channel. Each DSHOT frame from the FC gets acknowledged by a frame from the ESC containing the current eRPM. The FC needs to know the motor pole count to convert to RPM.

[The RPM filter](https://github.com/betaflight/betaflight/pull/7271) is a bank of 36 notch filters on gyro and Dterm which takes advantage of this high frequency RPM telemetry data to implement a motor harmonics filter which removes motor noise with surgical precision. In its default configuration it runs 12 notch filters each on pitch, roll, and yaw, covering the first 3 harmonics of each motor's RPM for the gyro filter bank.

These two features are currently supported by BLHeli_32 and require an update to the latest firmware. _See [References](#References)_

### Required configuration

Check the table at the bottom of this page to see if your FC is supported. Some boards require a custom build which can be found [here](https://ci.betaflight.tech/job/Joelucid%20dshot%20bidir%20test/lastSuccessfulBuild/artifact/obj/).

### Loop times and DSHOT protocol

Bidirectional DSHOT works with DSHOT 300, 600 and 1200, and also with Proshot 1000. Remember, though, that for each frame sent there will now be a frame coming back, and between input and output frames there is a period of 25us to switch the line, DMA, and timers. The loop time selection needs to be low enough that given the DSHOT protocol rate both frames + 50 us fit into one gyro loop iteration.

Both bidirectional DSHOT and the RPM filter are fairly CPU intensive and it is very important for the loop rates to be exactly on spot so that the filters get tuned to the right frequencies. It is recommended to run at 4k/4k. All DSHOT speeds should work at that loop rate.

On F4, RPM telemetry costs about 3-4uS per motor per line direction change. So something around 24-32uS for the line direction switching both directions together. The RPM filter has 36 notch filters that get dynamically tuned at 1000Hz update frequency. So running at 8k/8k can get tight.

First install the BLHeli_32 test firmware on your ESCs. Also switch off any extended startup melody since that currently interferes with bidirectional DSHOT. The standard startup tones will work fine though.

### DMA

The current implementation requires normal DMA to be used, not burst DMA. This may or may not work with a given FC. You can simply try it out:

``set dshot_burst=off``

And test whether your quad still flies. If so proceed to the next step:

### Enabling new scheduler policy

Since the RPM filter works with very narrow notch filters, it's imperative that the gyro loop time does not vary and is exactly as specified. This used to require low loop rates and overclocking. A scheduler change has now been added, which allows consistent gyro rates even at higher loop rates. Bidirectional DSHOT requires enabling this feature:

``set scheduler_optimize_rate=on``

### Enabling Bidirectional Dshot

``set dshot_bidir=on``

See if your motors still spin up. If so try detach the USB cable, connect a battery and reconnect USB. Now go to the CLI and type ``status``. You should see DSHOT telemetry being reported. The reported RPM should be zero for each motor and there should be few errors.

### Motor Poles

The ESCs report eRPM, which needs to be converted to RPM using the number of poles (magnets) of the motors. These are found on the bell of the motor, not the stator magnets where the windings are located. Typical 5" motors have 14 poles, so that is the default setting. Smaller motors have fewer poles, often 12. Count them or look up the motor specs and configure using:

``set motor_poles=14``

### Verifying consistent loop time

**Important:**

After enabling all of the above features double check that your loop rate is consistent. If not select a lower loop rate. Remember that unlike effective filtering, loop time has very minimal effect on [flight performance](https://github.com/betaflight/betaflight/issues/7327).

To do so enter ``status`` in the CLI and check that the gyro rate matches what you have specified.

### Debug modes

There are two blackbox debug modes to verify the RPM filter: RPM_FILTER logs the frequency of each motor as reported by the ESC. DSHOT_RPM_TELEMETRY logs the unconverted eRPM.

### Tuning (Sugar_K)

The RPM filter will do the heavy lifting without adding much latency. Typically only the Dterm lowpass filter and the dynamic notch are additionally needed to remove broad background noise and frame resonances, respectively. You should remove the filters in stages, test hovering and flying after each change to verify that your motors are not getting too hot.

The first thing to turn off is the stage2 Dterm lowpass filter.

``set dterm_lowpass2_hz = 0``

Next is to slim up the dynamic notch filter. We recommend setting dynamic notch width to 0, this gives you the classic single notch filter, not the cascaded version.

``set dyn_notch_width_percent = 0``

Now you need to set the minimum frequency and range for the dynamic notch filter. On a typical 5” racer, a dynamic min of 200hz and range of medium should be fine, but for a larger quad with 6-7” props, you might need to set the min to 100hz and range to low. A black box log will verify this, basically you are looking for the frequency of the idle motor noise.
```
set dyn_notch_range = medium
set dyn_notch_min_hz = 200
```
If the quad is very clean, you can likely get away with the dynamic notch Q of 200-250 (stock is 120). This makes the notch narrower and produce less latency.

``set dyn_notch_q = 200``

Next is gyro lowpass filtering. Remember there are both static and dynamic gyro lpfs, if you turn off the dynamic it will revert to a static lowpass and you need to turn that off too. 
```
set gyro_lowpass_hz = 0
set dyn_lpf_gyro_max_hz = 0
```

If your quad doesn't like this and your motors get too hot, reenable the static gyro lowpass filter and change its type to PT1 to at least reduce its latency by a small amount.
```
set gyro_lowpass_hz = 150
set gyro_lowpass_type = PT1
```
Lastly, you should be able to push the dynamic Dterm lowpass filtering up a bit higher, you need it so don’t turn it off, but you can increase the min/max and make some big savings to the overall filter latency. Stock settings for min/max are 150Hz and 250Hz, respectively. Starting with the max, you can try to push it to 300 or even 400Hz.

``set dyn_lpf_dterm_max_hz = 300``

Once that is set, you can try pushing the min up to 200Hz. If this is too high, you will get a grinding noise from your motors at idle, and if that happens, simply lower it back to 150Hz.

``set dyn_lpf_dterm_min_hz = 200``

It is possible to run the dynamic notch filter off if you lower your Dterm lowpass enough, but this removes any filtering that can react to external influences, e.g. wind. I had a near flyaway while testing this on a really windy day where the wind gusts were stirring up the motors enough to not descend on idle throttle.

Finally, I do recommend using the D only TPA (aka TDA, which is now the default in 4.0) using a strong rate cut of 60-80% (default 50%) and the threshold at 1750 (default 1500). This way, you still have strong D for pulling out of prop wash, but it also cuts nice and sharply for high throttle runs.
```
set tpa_rate = 60
set tpa_breakpoint = 1750
```

### Supported FCs

| Board | Target | dshot_bidir | Tester | Notes |
|---|--|---|---|---|
|       | OMNIBUSF4SD | OK | joelucid |
| Banggood 20mm NOXE | NOX | OK | joelucid |
| Kakute F4 V2 | | OK | bizmar |
| Airbot F4 Nano V6 | OMNIBUSF4FW | OK | skonk |
| MATEK F405-XXX | MATEKF405 | OK | Wudz_17 |
| CLRACINGF4 | CLRACINGF4 | OK | joelucid | requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch |
| REVOLTOSD | REVOLTOSD | OK | JayBird | requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch |
| FlameF4 | OMNIBUSF4 | OK | omerco | |
|OMNIBUSF4V6| OMNIBUSF4V6 | OK | Jack | Requires ESC FW 32.6.2 |
| AIKON F4 2020 | AIKONF4 | OK | fujin | requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch |
| Betaflight F4 | BETAFLIGHTF4 | OK | Balint | |
| [Hyperlite F4 OSD V1/V2](https://pyrodrone.com/products/hyperlite-f4-osd) | PYRODRONEF4 | OK | fujin | requires [pyrodrone-f4-bidir-support](https://github.com/fujin/betaflight/tree/pyrodronef4-bidir-support) branch |

Please add additional verified configurations here.

### References

[Bidirectional Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[Rpm Filter PR](https://github.com/betaflight/betaflight/pull/7271)

A test version of blheli32 with erpm telemetry on signal line support is now [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM). Download your ESC firmware for your target from `BLHeli_32 Test code Rev32.6.X hex files` folder where `X` is the current test revision.