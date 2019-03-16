
### DSHOT & Betaflight 4.0

[Bidirectional DSHOT](https://github.com/betaflight/betaflight/pull/7264) is a new feature in Betaflight 4.0 which enables the flight controller to receive high frequency RPM telemetry for each motor on the ESC motor signal line. It does not require any additional wiring or an additional telemetry back-channel. Each DSHOT frame from the FC gets acknowledged by a frame from the ESC containing the current eRPM. The FC needs to know the motor pole count to convert to RPM.

[The RPM filter](https://github.com/betaflight/betaflight/pull/7271) is a bank of 36 notch filters on gyro and Dterm which takes advantage of this high frequency RPM telemetry data to implement a motor harmonics filter which removes motor noise with surgical precision. In its default configuration it runs 12 notch filters each on pitch, roll, and yaw, covering the first 3 harmonics of each motor's RPM for the gyro filter bank.

These two features are currently supported by BLHeli_32 and require an update to the latest firmware. _See [References](#References)_

Here's a demo of the feature in flight. Quad has minimal filtering other than the rpm filter, handles very well and shows close to no prop wash: https://youtu.be/jwFYaGHp91c

## Configuration

### BLHeli32 firmware update

First install the [BLHeli_32 test firmware](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM) on your ESCs. To identify the file you need start blheli32 configurator and select the "flash Blheli" button. A window will open which shows the firmware name. Download the matching firmware and flash it onto **all four** ESCs. You will need to select the firmware separately for each ESC.

Also switch off any extended startup melody since that currently interferes with bidirectional DSHOT. The standard startup tones will work fine though.

### Motor Magnets

The ESCs report eRPM, which needs to be converted to RPM using the number of magnets of the motors. These are found on the bell of the motor, not the stator magnets where the windings are located. Typical 5" motors have 14 magnets, so that is the default setting. Smaller motors have fewer magnets, often 12. Count them or look up the motor specs and configure using the following command in the CLI if you don't have 14 magnets:

``set motor_poles=xx`` where xx is the number of magnets you counted.

### Config Snippet

Check the table at the bottom of this page to see if your target is supported. Some boards require a reconfiguration of timer or dma channels. Additionally since the rpm filter removes motor noise so effectively we have developed a set of filter defaults optimized for use with the rpm filter. Both changes plus conservative selections of DSHOT600 and a gyro frequency of 4k are included in a board specific snippet which is liked in the table.

Click on the snippet for your board and cut/paste the commands into the CLI. Paste the snippet into the CLI now if your board needs one.

Don't be discouraged if your target isn't listed. Many targets will work. Use this [Default Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf), try it out and report back.

### Config Verification

Now your FC is set up for bidirectional dshot. You now need to verify that it works. To do so power cycle FC and ESC. Connect the lipo first to the ESC, then the USB cable. Open the CLI and enter ``status``. You should now see bidirectional dshot statistics similar to this:

```
Dshot reads: 145267
Dshot invalid pkts: 36
Dshot irq micros: 5
Dshot RPM Motor 0: 0
Dshot RPM Motor 1: 0
Dshot RPM Motor 2: 0
Dshot RPM Motor 3: 0
```
The number of invalid packets should not exceed 1% of all Dshot reads. All motors should report an RPM of 0.

Type ``exit`` to leave the CLI. Go to the motors tab and let all motors spin very slowly. Go back to the CLI and repeat the ``status`` command. Now your output should look like this:

```
Dshot reads: 505108
Dshot invalid pkts: 8
Dshot irq micros: 4
Dshot RPM Motor 0: 106
Dshot RPM Motor 1: 112
Dshot RPM Motor 2: 107
Dshot RPM Motor 3: 111
```

If so you're ready for your first test flight! Log to blackbox if you can. The snippet sets the debug_mode to rpm_filter which allows you to see the live rpm of your quad in your blackbox log.

## Advanced Topics

If your board is supported the board snippet should get you up in the air with very effective default settings. Below is a description of more advanced topics which explains some of the setting in the config snippets.

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

To do so enter ``tasks`` in the CLI and check that the gyro rate matches what you have specified (Note:  Battery should be connected to FC to get accurate loop rate results). For example:

```
# tasks
Task list             rate/hz  max/us  avg/us maxload avgload     total/ms
00 - (         SYSTEM)     10       1       0    0.5%    0.0%         0
01 - (         SYSTEM)   1000       3       1    0.8%    0.6%       522
02 - (       GYRO/PID)   7999      43      34   34.8%   27.6%      2845
03 - (            ACC)   1000      12      10    1.7%    1.5%       107
04 - (       ATTITUDE)    100      17      10    0.6%    0.6%        11
05 - (             RX)     32      34      32    0.6%    0.6%        12
06 - (         SERIAL)    100     851       3    9.0%    0.5%         8
08 - (BATTERY_VOLTAGE)     50       4       2    0.5%    0.5%         1
09 - (BATTERY_CURRENT)     50       1       1    0.5%    0.5%         0
10 - ( BATTERY_ALERTS)      5       3       2    0.5%    0.5%         0
11 - (         BEEPER)    100       2       1    0.5%    0.5%         1
14 - (           BARO)     43      98      66    0.9%    0.7%        34
15 - (       ALTITUDE)     40       7       3    0.5%    0.5%         1
17 - (      TELEMETRY)    250       1       0    0.5%    0.0%        27
19 - (            OSD)     60      21      13    0.6%    0.5%         9
21 - (            CMS)     60       1       1    0.5%    0.5%         0
22 - (        VTXCTRL)      5       1       1    0.5%    0.5%         0
23 - (        CAMCTRL)      5       1       1    0.5%    0.5%         0
25 - (    ADCINTERNAL)      2       3       1    0.5%    0.5%         0
26 - (       PINIOBOX)     19       1       1    0.5%    0.5%         0
RX Check Function                   2       1                         0
Total (excluding SERIAL)                        46.0%   37.1%
```

You need to check the *GYRO/PID* line:

```
02 - (       GYRO/PID)   7999      43      34   34.8%   27.6%      2845
```

In this case we have the Gyro/PID configured in 8k/8k and this line show us that it is executing at a rate of 7999Hz. This must be very, very close to the 8k value (8000Hz). A recommendation will be maintain the error under the 1%.

### Debug modes

There are two blackbox debug modes to verify the RPM filter: RPM_FILTER logs the frequency of each motor as reported by the ESC. DSHOT_RPM_TELEMETRY logs the unconverted eRPM.

### Tuning (Sugar_K)

The RPM filter will do the heavy lifting without adding much latency. Typically only the Dterm lowpass filter and the dynamic notch are additionally needed to remove broad background noise and frame resonances, respectively. **You should remove the filters in stages, test hovering and flying after each change to verify that your motors are not getting too hot.**
running quick black box logs and looking at the gyro spectra graphs to see if you suddenly gain any massive noise spikes is also a good idea and more useful that just running Plasma Tree graphs as your PT graphs are going to look worse as you turn stuff off but this is to be expected as you reduce the overall filter levels.
please also note the absolute numbers and number of filters turned off were reached using a very clean high power to weight racing quad with good props...
Lastly  **do not turn off all the Dterm filtering, doing this is a very bad idea**

**The first thing to do is to get a good 4.0 tune**.
I don't recommend dumping in some one else filtering settings with out first tuning your quad on 4.0 as it is. basically running RPM filtering in its optimised state means you have very little actual filtering and for this to work well you are going to need a mechanically sound build that doesn't  have de-lamitated arm, loose bolts, a bad gyro chip or cooked motor bearing. by starting with a quad tuned with out the RPM filtering you can simply move to turning off the un needed filters and then optimise them a little.

Also its come to light that the way rc smoothing is set up right now is not entirely ideal and can cause FF to drive your motors up and down as the Fc receives packets from openTX with zeros. 

first thing to do if you run hall sensor gimbals ( frsky radios ) is go to the hardware tab in the radio menu and uncheck the ADC filter box, for this to work you need to change your dead band in the radio menu to 0 on all axis ( this is pretty important)
then set your smoothing filters on input and derivative to PT1.
a good starting point for the filter settings is..

```set rc_smoothing_input_hz = 40
set rc_smoothing_derivative_hz = 100
set rc_smoothing_input_type = PT1
set rc_smoothing_derivative_type = PT1```

there are some other fixes coming but they will be in BF 4.1



 now on to the tuning of filtering using the Bi Directional Dshot 

remember to test fly every single stage of this process 

first filter to  turn off is the stage2 Dterm lowpass filter.

``set dterm_lowpass2_hz = 0``

Next is to slim up the dynamic notch filter. We recommend setting dynamic notch width to 0, this gives you the classic single notch filter, not the cascaded version.

``set dyn_notch_width_percent = 0``

Now you need to set the minimum frequency and range for the dynamic notch filter. On a typical 5” racer, a dynamic min of 200hz and range of medium should be fine, a heavier acro quad 150hz would be a good start  but for a larger quad with 6-7” props, you might need to set the min to 100hz and range to low. A black box log will verify this, basically you are looking for the frequency of the idle motor noise.
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

**UPDATE I'm leaning towards a min of 150hz**


It is possible to run the dynamic notch filter off if you lower your Dterm lowpass enough, but this removes any filtering that can react to external influences, e.g. wind. I had a near flyaway while testing this on a really windy day where the wind gusts were stirring up the motors enough to not descend on idle throttle.

Finally, I do recommend using the D only TPA (aka TDA, which is now the default in 4.0) using a strong rate cut of 60-80% (default 50%) and the threshold at 1750 (default 1500). This way, you still have strong D for pulling out of prop wash, but it also cuts nice and sharply for high throttle runs.
```
set tpa_rate = 80
set tpa_breakpoint = 1750
```

### Supported targets

| Target | Changes required | Notes | Supported Motors|
| --- | --- | --- | --- |
| AG3XF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf) |  | M1 - M4 (tested Mister_M) |
| AIKONF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/AIKONF4.cf) |  | M1 - M4 (tested fujin) |
| ALIENFLIGHTNGF7 | [snippet](https://github.com/betaflight/bidircfg/blob/master/ALIENFLIGHTNGF7.cf) | M3 doesn't work, use one of M5-9 instead. LED doesn't work with M1 | M1-M2, M4-M9 |
|ALIENWHOOP|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)|| M1-M4 | 
|ANYFCF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1 M2 M3 M4 M5 M6 M9|
|ANYFCM7|[snippet](https://github.com/betaflight/bidircfg/blob/master/ANYFCM7.cf)||M1 M2 M3 M4 M5 M7 M9 M10|
|BETAFLIGHTF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)| | M1 - M4 ok (tested Balint) |
|CLRACINGF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/CLRACINGF4.cf)| | M1-M4 ok|
|CLRACINGF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)|Motor 4 doesn't work. Use the LED pad instead|M1 M2 M3 M5|
|DALRCF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/DALRCF4.cf)||M1-M6 (tested QuadMcFly)|
|DALRCF722DUAL|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M6. But either M5 or M6|
|DYSF4PRO|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4 (tested BRadFPV)|
|ELINF405|[snippet](https://github.com/betaflight/bidircfg/blob/master/REVOLT.cf)||M1-M4 (tested elin-neo)|
|EXF722DUAL|[snippet](https://github.com/betaflight/bidircfg/blob/master/EXF722DUAL.cf)||M1-M8|
|FLYWOOF7DUAL|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M6|
|FORTINIF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4(tested QuadMcFly)|
|FOXEERF722DUAL|[snippet](https://github.com/betaflight/bidircfg/blob/master/FOXEERF722DUAL.cf)||M1-M6|
|FURYF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4|
|HAKRCF722|[snippet](https://github.com/betaflight/bidircfg/blob/master/HAKRCF722.cf)||M1-M6|
|KAKUTEF4V2 | [snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf) |  ||M1-M4 tested|
|KISSFCV2F7|[snippet](https://github.com/betaflight/bidircfg/blob/master/KISSFCV2F7.cf)||M1-M6|
|MATEKF405|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4 tested (Wudz_17)|
|MATEKF722|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M8|
|MATEKF722SE|[snippet](https://github.com/betaflight/bidircfg/blob/master/MATEKF722SE.cf)|M5 does not work|M1-M4, M6-M8|
|NERO|[snippet](https://github.com/betaflight/bidircfg/blob/master/NERO.cf)||M1-M8|
|NOX|[snippet](https://github.com/betaflight/bidircfg/blob/master/NOX.cf)||M1-M4|
|NUCLEOF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)|M4 does not work but can be replaced with M6|M1-M3,M6|
|NUCLEOF722|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)|M4 does not work but can be replaced with M6|M1-M3,M6|
|OMNIBUSF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4 (tested omerco)|
|OMNIBUSF4SD|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4 (tested joe lucid)|
|OMNIBUSF4FW|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)| | M1-M4 tested (skonk) |
|OMNIBUSF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/OMNIBUSF7.cf)||M1-M4|
|OMNINXTF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M4|
|PYRODRONEF4|[snippet](https://github.com/betaflight/bidircfg/blob/master/PYRODRONEF4.cf)||M1-M4 (tested fujin)|
|REVOLTOSD|[snippet](https://github.com/betaflight/bidircfg/blob/master/REVOLT.cf)||M1-M4 (tested JayBird)|
|SPRACINGF7DUAL|[snippet](https://github.com/betaflight/bidircfg/blob/master/SPRACINGF7DUAL.cf)||M1-M10|
|YUPIF7|[snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf)||M1-M6|

Please add additional verified configurations here.

### References

[Bidirectional Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[Rpm Filter PR](https://github.com/betaflight/betaflight/pull/7271)

A test version of blheli32 with erpm telemetry on signal line support is now [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM). Download your ESC firmware for your target from `BLHeli_32 Test code Rev32.6.X hex files` folder where `X` is the current test revision.