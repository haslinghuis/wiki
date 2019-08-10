### UPDATE: Bidirectional DSHOT and RPM Filter now available on BLHeli_S escs. [Early Access here!](https://jflight.net) ###

## Introduction

RPM based notch filtering results in far more effective removal of motor noise from the gyro data than ever before, with less delay.  The dynamic notch filter then doesn't need to be so wide, and is freed up to more effectively chase and eliminate other frame resonances at frequencies other than motor rpm.  Together they provide a much more effective solution for noise management and bent props than ever before.  In most cases, enabling rpm filtering will mean that the lowpass filters can be shifted higher, or in some cases turned off completely, further reducing delay and improving propwash.  This should only be done by experienced users after reading the [tuning guide](#Tuning).

[Bidirectional DSHOT](https://github.com/betaflight/betaflight/pull/7264) is a new feature in Betaflight 4.1 which lets the flight controller receive high frequency RPM telemetry for each motor on the ESC motor signal line. It does not require any additional wiring or an additional telemetry back-channel. Each DSHOT frame from the FC gets acknowledged by a frame from the ESC containing the current eRPM. The FC needs to know the motor pole count to convert to RPM.  

[The RPM filter](https://github.com/betaflight/betaflight/pull/7271) is a bank of 36 notch filters on gyro and Dterm which takes advantage of this high frequency RPM telemetry data to implement a motor harmonics filter which removes motor noise with surgical precision. By default configuration it runs 12 notch filters each on pitch, roll, and yaw, covering the first 3 harmonics of each motor's RPM for the gyro filter bank.

These two features are currently supported by betaflight 4.1 with BLHeli_32 ESCs that have been updated to the latest 'GCR' firmware and BLHeli-S ESCs that have been flashed using [JFlight](https://jflight.net).

Earlier versions for betaflight 4.0 required different ESC code.  The use of 4.1 and GCR code is strongly recommended. _See [References](#References)_

Here's a demo in flight. Quad has minimal filtering other than the rpm filter, handles very well and shows close to no prop wash: 
https://youtu.be/jwFYaGHp91c, https://youtu.be/SoG245vmaLo

### Arming Prevention Check

If the RPM Filter is enabled but one or more of the ESC's are not supplying valid telemetry data, arming will be prevented, and the `RPMFILTER` message will be shown on the OSD (see [Arming Sequence & Safety](Arming-Sequence-&-Safety)). This prevents arming with incomplete or non-working configurations that may result in flyaways or hot motors. See below to ensure the ESC's are properly configured to support this feature.

## ESC Configuration

### Bidirectional DShot Firmware

For RPM Filtering to work, the ESC must support the Bidirectional DShot protocol, and Bidirectional DShot must be enabled in the CLI.  

The Bidirectional DShot protocol is different in 4.1 from 4.0.  In 4.1 a GCR encoding scheme is used for the return data.  It is important that the ESC code is correct for the version of Betaflight you are running. 

For BLHeli_32, the hex file is not available in the configurator, and must be downloaded separately and selected in the flashing interface. BetaFlight 4.0.x requires the [BLHeli_32 32.6.4](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM/BLHeli_32%20Test%20code%20Rev32.6.4%20hex%20files) firmware. BetaFlight 4.1 requires the more recent [32.6.6 firmware](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM/BLHeli_32%20Test%20code%20Rev32.6.4%20hex%20files/Rev32.6.6%20for%20inverted%20bidirectional%20dshot%20and%20GCR%20return%20data%20only). 

Download the correct firmware for your ESC brand/model by right clicking on "Raw" and choosing "Save As", and flash it onto **all four** ESCs. You will need to select the firmware file separately for each ESC as it will default to the most recent stable version of BLHeli.

Remove any extended startup melody if you have configured one for the ESCs since that currently interferes with Bidirectional DSHOT. The standard startup tones will work fine.

For JESC and 4.1, flash the latest GCR version to your BLHeli-S ESC/s using the JESC BLHeli configurator.

## Betaflight Configuration

### Motor Magnets

The ESCs report eRPM, which needs to be converted to RPM using the number of magnets of the motors. These are found on the bell of the motor, not the stator magnets where the windings are located. Typical 5" motors have 14 magnets, so that is the default setting. Smaller motors have fewer magnets, often 12. Count them or look up the motor specs and configure using the following command in the CLI if you don't have 14 magnets:

``set motor_poles=xx`` where xx is the number of magnets you counted.

### Config Snippet

The easiest way to make the required changes in Betaflight is to copy and paste the relevant snippet, from the list below, into the CLI, and saving.

Don't be discouraged if your target isn't listed. Many targets will work. Use this [Default Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT.cf), try it out and report back.

Beware of issue https://github.com/betaflight/betaflight/issues/8019: a diff of the config after applying the snippet will currently not cleanly reproduce the config when applied to a clean install. To fix this take any ```dma pin``` lines in the snippet and reapply them after applying your diff.

### Snippets for supported targets

| Target | Install Snippet | Notes | Supported Motors|
| --- | --- | --- | --- |
| AG3XF4| [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1 - M4 (tested Mister_M) |
| AIKONF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/AIKONF4-upgrade.cf) |  | M1 - M4 (tested fujin) |
| ALIENFLIGHTNGF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/ALIENFLIGHTNGF7-upgrade.cf) | M3 doesn't work, use one of M5-9 instead. LED doesn't work with M1 | M1-M2, M4-M9 |
|ALIENWHOOP|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)|| M1-M4 | 
|ANYFCF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1 M2 M3 M4 M5 M6 M9|
|ANYFCM7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/ANYFCM7-upgrade.cf)||M1 M2 M3 M4 M5 M7 M9 M10|
|BETAFLIGHTF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)| | M1 - M4 ok (tested Balint) |
|CLRACINGF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/CLRACINGF4-upgrade.cf)| | M1-M4 ok|
|CLRACINGF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)|Motor 4 doesn't work. Use the LED pad instead|M1 M2 M3 M5|
|DALRCF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DALRCF4-upgrade.cf)||M1-M6 (tested QuadMcFly)|
|DALRCF722DUAL|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DALRCF722DUAL-upgrade.cf)||M1-M6. But either M5 or M6|
|DYSF4PRO|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested BRadFPV)|
|ELINF405|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested elin-neo)|
|ELINF722|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested elin-neo)|
|EXF722DUAL|[Snippet](https://github.com/betaflight/bidircfg/blob/master/EXF722DUAL-upgrade.cf)||M1-M8|
|FLYWOOF7DUAL|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M6|
|FORTINIF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4(tested QuadMcFly)|
|FOXEERF722DUAL|[Snippet](https://github.com/betaflight/bidircfg/blob/master/FOXEERF722DUAL-upgrade.cf)||M1-M6|
|FURYF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/FURYF4SD-upgrade.cf)||M1-M4, No LED support, Tested RawFPV|
|FURYF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4|
|HAKRCF722|[Snippet](https://github.com/betaflight/bidircfg/blob/master/HAKRCF722-upgrade.cf)||M1-M6|
|KAKUTEF4V2|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  ||M1-M4 tested|
|KISSFCV2F7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/KISSFCV2F7-upgrade.cf)||M1-M6|
|LUXF4OSD|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  ||M1-M4 tested (Mister_M)|
|MAMBAF722|[Snippet](https://github.com/betaflight/bidircfg/blob/master/MAMBAF722-upgrade.cf)||M1-M4 tested (kc10kevin)|
|MATEKF405|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 tested (Wudz_17)|
|MATEKF722|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M8|
|MATEKF722SE|[Snippet](https://github.com/betaflight/bidircfg/blob/master/MATEKF722SE-upgrade.cf)|M5 does not work|M1-M4, M6-M8|
|MLTEMPF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested RC2 monko760)|
|NERO|[Snippet](https://github.com/betaflight/bidircfg/blob/master/NERO-upgrade.cf)||M1-M8|
|NOX|[Snippet](https://github.com/betaflight/bidircfg/blob/master/NOX-upgrade.cf)||M1-M4|
|NUCLEOF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)|M4 does not work but can be replaced with M6|M1-M3,M6|
|NUCLEOF722|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)|M4 does not work but can be replaced with M6|M1-M3,M6|
|OMNIBUSF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested omerco)|
|OMNIBUSF4SD|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4 (tested joe lucid)|
|OMNIBUSF4FW|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)| | M1-M4 tested (skonk) |
|OMNIBUSF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/OMNIBUSF7-upgrade.cf)||M1-M4 (tested in RC2 IgguT)|
|OMNINXTF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M4|
|PYRODRONEF4|[Snippet](https://github.com/betaflight/bidircfg/blob/master/PYRODRONEF4-upgrade.cf)||M1-M4 (tested fujin)|
|REVOLTOSD|[Snippet](https://github.com/betaflight/bidircfg/blob/master/REVOLT-upgrade.cf)||M1-M4 (tested JayBird)|
|SPRACINGF7DUAL|[Snippet](https://github.com/betaflight/bidircfg/blob/master/SPRACINGF7DUAL-upgrade.cf)||M1-M10|
|YUPIF7|[Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf)||M1-M6|

Please add additional verified configurations here.

### Unsupported targets

| Target | Notes |
| --- | --- |


### Config Verification

Now your FC is set up for bidirectional dshot, let's verify that it works. To do so power cycle FC and ESC. Connect the lipo first to the ESC, then the USB cable.  Open the CLI and enter ``status``. For BF 4.1 builds use the command ``dshot_telemetry_info`` instead. You should now see bidirectional dshot statistics similar to this:

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

Type ``exit`` to leave the CLI. Go to the motors tab, click the safety toggle, move the slider up, and let all motors spin very slowly. Go back to the CLI and repeat the ``status`` command. Now your output should look like this:

```
Dshot reads: 505108
Dshot invalid pkts: 8
Dshot irq micros: 4
Dshot RPM Motor 0: 106
Dshot RPM Motor 1: 112
Dshot RPM Motor 2: 107
Dshot RPM Motor 3: 111
```
**Note that the motors have to actually be spinning at the time you check with the `status` command for a non-zero RPM to be reported.**

Now you're ready for your first test flight! Log to blackbox if you can. The snippet sets the debug_mode to rpm_filter which allows you to see the live rpm of your quad in your blackbox log.  For later logs, and to test filters for tuning, change this to `set debug_mode = GYRO_SCALED`


## Advanced Topics

If your board is supported the board snippet should get you up in the air with very effective default settings. Below is a description of more advanced topics which explains some of the setting in the config snippets.

### Loop times and DSHOT protocol

Bidirectional DSHOT works with DSHOT 300, 600 and 1200, and also with Proshot 1000. 

For practical purposes, DShot 300 works well at all PID loop rates.

Remember that for each frame sent there will now be a frame coming back, and between input and output frames there is a period of 25us to switch the line, DMA, and timers. The loop time selection needs to be low enough that given the DSHOT protocol rate both frames + 50 us fit into one gyro loop iteration.

Both bidirectional DSHOT and the RPM filter are fairly CPU intensive and it is very important for the loop rates to be exactly on spot so that the filters get tuned to the right frequencies. It is recommended to run at 4k/4k with DShot 300 initially. All DSHOT speeds should work at that loop rate.  

On F4, RPM telemetry costs about 3-4uS per motor per line direction change. So something around 24-32uS for the line direction switching both directions together. The RPM filter has 36 notch filters that get dynamically tuned at 1000Hz update frequency. So running at 8k/8k can get tight.

First install the ESC firmware on your ESCs. Switch off any extended startup melody since that currently interferes with bidirectional DSHOT. The standard startup tones will work fine though.

### DMA

The current implementation requires normal DMA to be used, not burst DMA. Turning burst DMA off may, of itself, not work with a given FC. You can try it out in advance:

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

## Tuning

The RPM filter will do the heavy lifting of removing nearly all motor noise without adding much latency. 

However, general 'junk' noise from bearings, wind and turbulence will not be removed by the RPM filters;   lowpass filtering will still be required to control those noise elements.  

Frame resonances, which manifest as large fixed frequency noise lines, won't be removed by the RPM filters either; they are probably best managed by keeping the Dynamic Notch.  

The default filters in 4.1 are different from 4.0.  They have been designed to work well with RPM filtering with no changes, and should be used when first flying with the rpm filter.  Pilots trying out rpm filtering with 4.0x should set their filters to the 4.1 defaults when trying out rpm filtering using the snippet below, but we recommend starting rpm with 4.1 and a clean install.

```
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 150
set dyn_lpf_gyro_min_hz = 200
set dyn_lpf_gyro_max_hz = 500
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 250

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 100
set dyn_lpf_dterm_min_hz = 70
set dyn_lpf_dterm_max_hz = 170
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 150
```

Filter delay can be reduced by:
- converting biquad filters to PT1s, 
- lifting the lowpass filter cutoff number to a higher value, or 
- by disabling the entire filter bank (setting its cutoff value to zero).  

Each of these actions lets more noise through the filter bank to the PIDs and then to the motors.  

## WARNING:  Removing entire lowpass filter blocks can result in hot or burnt out motors, or flyaways!

**Filters should only ever be changed one at a time, test hovering and then test flying after each individual change to verify that your motors are not getting too hot.**

Black box logging, before and after, and using PID Toolbox to look at the spectral graphs is useful to know which filters aren't needed and to check that you haven't suddenly gained any massive noise spikes. 

ALWAYS save a `diff all` listing of your starting filter settings before changing anything, so you can go back. 

ALWAYS do your first test flight with the standard 4.1 filter settings!  The only exception to this rule is where  the current filter settings have been carefully tuned beforehand are known to be good.  

Note that adding rpm filtering using a snippet *will not* change your exiting lowpass filter settings, and will not cause flyways of itself.  It will narrow the dynamic notch to focus it better on chasing frame resonances, but that of itself is likely not to be bad.

## Lifting lowpass filters after a successful test flight

If the test flights are positive - motors cool, no bad noises on arming, etc - you may be able to improve propwash by lifting filter cutoffs or disabling filters.

My personal preference for reducing lowpass delay on clean builds is to simply move all 4.1 filters to higher values, keeping their relative values the same.  For instance, the following snippet shifts all 4.1 lowpass filters up by about 50%, and cuts delay by a couple of milliseconds:

 ```
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 225
set dyn_lpf_gyro_min_hz = 300
set dyn_lpf_gyro_max_hz = 900
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 350

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 150
set dyn_lpf_dterm_min_hz = 100
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 200
```

I recommend the above as the first step to reduce filtering delay.  It is quite a big step up and should not be done unless you are sure the build is good.  On first arming, if the quad makes a grinding noise or shows random tendencies to jump up on arming or during hover, that usually means you've gone up to high (on D mostly).

I do not recommend going higher unless you are an expert and have blackbox logging capabilities, or like living dangerously.  However the next step would be something like 1.75x defaults, and then even 2x defaults.  If the lowpass filters are set twice as high as defaults, you sure better have a really clean quad - but you'll have halved your lowpass filter delay.

## Configuring the Dynamic Notch with RPM filtering

The Dynamic Notch range is by default set to AUTO mode.  This uses the value of dyn_lpf_gyro_max_hz to choose the frequency range over which the dynamic notch will operate.  

Because frame resonances usually happen in the LOW or MEDIUM dynamic notch range, it is best, when rpm filtering is used, to configure the Dynamic Notch manually into LOW range mode.  If you know you have no frame resonances below 150hz, choosing MEDIUM will reduce delay and keep the dynamic notch higher.  

If you want the dynamic notch to stay above a certain minimum value, because you know you have no resonances below that value and want it to target something higher up, set the dynamic notch minimum frequency accordingly (just below the resonance that you are targeting)

For example, this snippet would let the dynamic notch run between 90 to about 330hz with a narrow single notch:
```
set dyn_notch_range = low
set dyn_notch_min_hz = 90
set dyn_notch_q = 200
set dyn_notch_width = 0
```
This snippet would let the dynamic notch range from 180 to about 550hz with a narrow single notch:
```
set dyn_notch_range = medium
set dyn_notch_min_hz = 180
set dyn_notch_q = 200
set dyn_notch_width = 0
```

If the resonance line is very narrow, you can likely get away with the dynamic notch Q of 250.  Wider or more diffuse resonance bands may need a Q of 120-150.  

It is always best to compare spectrums with dynamic filter on or off to check exactly what it is doing.

It is possible to turn the dynamic notch filter off altogether on stiff resonance-free builds, but this should be done with caution, and is best tested by before and after logging confirms that it is not contributing meaningfully to the end result. 

## Disabling low-pass filters completely

To disable lowpass filters, without cooking your motors, you are going to need a mechanically sound build that doesn't have de-laminated arms, loose bolts, a bad gyro chip or noisy motor bearings. 

Disabling filters have fairly substantial effects and should only be done by people familiar with filtering.  Generally it is safer to move filters up or down as a group.

The following will disable the first gyro lowpass completely:
```
set gyro_lowpass_hz = 0
set dyn_lpf_gyro_max_hz = 0
```

This will disable the second gyro lowpass completely:
```
set gyro_lowpass2_hz = 0
```

## TPA

This snippet will set a 4.0x build to the 4.1 default of 65% D only TPA starting at 1250
```
set tpa_rate = 65
set tpa_breakpoint = 1250
```

#### Note: The RC Smoothing fix for FrSky section has been moved to the [4.0 Tuning Notes](4.0-Tuning-Notes#bonus-section-rc-smoothing-fix-for-frsky-transmitters) page.

### References

[Bidirectional Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[Rpm Filter PR](https://github.com/betaflight/betaflight/pull/7271)

A test version of blheli32 with erpm telemetry on signal line support is now [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM). Download your ESC firmware for your target from `BLHeli_32 Test code Rev32.6.X hex files` folder where `X` is the current test revision.