### FAQ

- When will bidirectional DSHOT come to non F7 and F4 FC's with n channels or incompatibilities without burst?

We're currently investigating an alternative implementation approach using GPIO DMA which would likely work on the vast majority of FCs and would then be the one to port to F7.

### Dshot & Betaflight 4.0

[Bidirectional Dshot](https://github.com/betaflight/betaflight/pull/7264) is a new feature in Betaflight 4.0 which enables the flight controller to receive high frequency rpm telemetry for each motor on the ESC motor signal line. It does not require any additional wiring or an additional telemetry back-channel. Each dshot frame from the FC gets acknowledged by a frame from the ESC containing the current eRPM. The FC needs to know the motor pole count (number of magnets) to convert to rpm.

[The rpm filter](https://github.com/betaflight/betaflight/pull/7271) is a bank of 36 notch filters on gyro and dterm which takes advantage of this high frequency rpm telemetry data to implement a motor harmonics filter which removes motor noise with surgical precision. In its default configuration it runs 12 notch filter on pitch, roll and yaw, each, covering the first 3 harmonics of each motor's rpm for the gyro filter bank.

These two features are currently supported by blheli32 and require an update. _See [References](#References)_

### Required configuration

Check the table at the bottom of this page to see if your FC is supposed. Some boards require a custom build which can be found [here](https://ci.betaflight.tech/job/Joelucid%20dshot%20bidir%20test/lastSuccessfulBuild/artifact/obj/).

### Loop times and dshot protocol

Bidirectional dshot works with dshot 300, 600 and 1200; and with proshot 1000. Remember though that for each frame sent there will now be a frame coming back. And between input and output frames there is a period of 25us to switch the line, dma and timers. The loop time needs to be selected low enough that given the dshot protocol rate both frames + 50 us fit into one gyro iteration. 

Both bidirectional dshot and the rpm filter are fairly cpu intensive and it is very important for the loop rates to be exactly on spot so that the filters get tuned to the right frequencies. I recommend running at 4k/4k. All dshot speeds should work at that loop rate. 8k/8k has also been used successfully. Try it once you've successfully mastered 4k.

On F4 rpm telemetry costs about 3-4 uS per motor per line direction change. So something around 24-32 uS for the line direction switching both directions together. And the rpm-filter has 36 notch filters that get dynamically tuned at 1000 hz update frequency. So running at 8k/8k can get tight.

First install the blheli32 test fw on your escs. Also switch off any extended startup melody since that currently interferes with bidirectional dshot. The standard startup tones will work fine though.

### DMA

The current implementation requires normal DMA to be used, not burst dma. This may or may not work with a given FC. You can simply try it out:

``set dshot_burst=off``

And test whether your quad still flies. If so proceed to the next step:

### Enabling new scheduler policy

Since the rpm filter works with very narrow notch filters it's imperative that the gyro loop time does not vary and is exactly as specified. This used to require low loop rates and overclocking. A scheduler change has now been added, which allows consistent gyro rates even at higher loop rates. Bidirectional Dshot requires enabling this feature:

``set scheduler_optimize_rate=on``

### Enabling Bidirectional Dshot

``set dshot_bidir=on``

See if your motors still spin up. If so try detach the USB cable, connect a battery and reconnect USB. Now go to the CLI and type ``status``. You should see dshot telemetry being reported. The reported rpm should be zero for each motor and there should be few errors.

### Motor Poles

The escs report erpm which needs to be converted to rpm using the number of poles (magnets) of the motors. Regular 5" motors have 14 poles and that's the default setting. Smaller motors have less poles, often 12. Count them or look up the motor specs and configure using:

``set motor_poles=14``

### Verifying consistent loop time

**Important:**

After enabling all of the above features double check that your loop rate is consistent. If not select a lower loop rate. Remember that unlike effective filtering loop time has very minimal effect on [flight performance](https://github.com/betaflight/betaflight/issues/7327).

To do so enter ``status`` in the CLI and check that the gyro rate matches what you have specified.

### Debug modes

There are two blackbox debug modes to verify the rpm filter: RPM_FILTER logs the frequency of each motor as reported by the esc. DSHOT_RPM_TELEMETRY logs the unconverted erpm.

### Tuning (Sugar_K)

The rpm filter will do the heavy lifting without adding much latency. Typically only the dterm lpf and the dynamic notch are additionally needed to remove broad background noise and frame resonances respectively. You should remove the filters in stages.

The first thing to turn off is the stage2 Dterm lpf filter. Next is to slim up the dynamic notch filter. We recommend setting your dynamic width to 0, this gives you a single notch filter not the cascaded version 
```
set dyn_notch_width_percent = 0
```
Now you need to set the min freq for the dynamic and the range. basically you need the dynamic to clean up after the rpm filter. on a 5” racer a dynamic min of 200hz and range of medium should be fine, for a long ranger with 6-7” props you might need to set the min to 100hz and low. A black box log will show you what you need to know. basically you are looking for the frequency of the idle motor noise. If the quad is very clean you can likely get away with the dynamic notch Q of 200-250 ( stock is 120 ) this makes the notch narrower and produces less latency 
the next thing to turn off. Remember to test hover and fly each setting change.

Next is gyro LPF filtering. Remember there are both static and dynamic gyro lpfs, if you turn off the dynamic it will revert to the static so you need to turn that off too. 
```
set gyro_lowpass_hz = 0
set dyn_lpf_gyro_min_hz = 0
```

If the quad doesn't like this i recommend changing the type of LPF to a PT1 to get some gain in latency over the Biquad. Lastly you should be able to push the dynamic dterm LPF filtering, you need it so don’t turn it off but you can increase the min/max and make some big savings to the over all filter latency.

First i would start with the max, stock its 250hz but 300-400 should be ok, also the min can be pushed. If you get this wrong you will get motor grinding at idle so its easy to tune. If you can push this up to 200hz your pretty much golden.

Lastly is it possible to run the dynamic notch off if you lower your Dterm lpf enough but this removes any filtering that can react to external influences e.g. wind. I had a near fly away testing this on a really windy day where the wind gusts were stirring up the motors enough too not descend on idle throttle.

Finally I do recommend using the D only TPA aka TDA using a strong cut of 60-80% and the threshold at 1750 so you still have strong D for pulling out of prop wash but then it cuts nice and sharply for high throttle runs.

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
| | OMNIBUSF4 | OK | omerco | |
|OMNIBUSF4V6| OMNIBUSF4V6 | OK | Jack | Requires ESC FW 32.6.2 |
| AIKON F4 2020 | AIKONF4 | OK | fujin | requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch |
| Betaflight F4 | BETAFLIGHTF4 | OK | Balint | |
| [Hyperlite F4 OSD V1/V2](https://pyrodrone.com/products/hyperlite-f4-osd) | PYRODRONEF4 | OK | fujin | requires [pyrodrone-f4-bidir-support](https://github.com/fujin/betaflight/tree/pyrodronef4-bidir-support) branch |

Please add additional verified configurations here.

### References

[Bidirectional Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[Rpm Filter PR](https://github.com/betaflight/betaflight/pull/7271)

A test version of blheli32 with erpm telemetry on signal line support is now [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM). Download your ESC firmware for your target from `BLHeli_32 Test code Rev32.6.X hex files` folder where `X` is the current test revision.