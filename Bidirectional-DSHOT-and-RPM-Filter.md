### FAQ

- When will bidirectional DSHOT come to non F7 and F4 FC's with n channels or incompatibilities without burst?

We're currently investigating an alternative implementation approach using GPIO DMA which would likely work on the vast majority of FCs and would then be the one to port to F7.

### Dshot & Betaflight 4.0

[Bidirectional Dshot](https://github.com/betaflight/betaflight/pull/7264) is a new feature in Betaflight 4.0 which enables the flight controller to receive high frequency rpm telemetry for each motor on the ESC motor signal line. It does not require any additional wiring or an additional telemetry back-channel. Each dshot frame from the FC gets acknowledged by a frame from the ESC containing the current eRPM. The FC needs to know the motor pole count (number of magnets) to convert to rpm.

[The rpm filter](https://github.com/betaflight/betaflight/pull/7271) is a bank of 48 notch filters on gyro and dterm which takes advantage of this high frequency rpm telemetry data to implement a motor harmonics filter which removes motor noise with surgical precision. In its default configuration it runs 12 notch filter on pitch, roll and yaw, each, covering the first 3 harmonics of each motor's rpm for the gyro filter bank and the first harmonic for the dterm.

These two features are currently supported by blheli32 and require an update. A first test version is [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM/BLHeli_32%20Test%20code%20Rev32.6.1%20hex%20files).

### Required configuration

### Loop times and dshot protocol

Bidirectional dshot works with dshot 300, 600 and 1200; and with proshot 1000. Remember though that for each frame sent there will now be a frame coming back. And between input and output frames there is a period of 25us to switch the line, dma and timers. The loop time needs to be selected low enough that given the dshot protocol rate both frames + 50 us fit into one gyro iteration. 

Decoding of proshot packets is significantly faster than dshot. ~~If it works on your FC we recommend using proshot 1000 for that purpose.~~ There are currently issues with proshot, so we recommend the use of dshot until these get resolved.

Both bidirectional dshot and the rpm filter are fairly cpu intensive and it is very important for the loop rates to be exactly on spot so that the filters get tuned to the right frequencies. I recommend running at 4k/4k. All dshot speeds should work at that loop rate.

On F4 rpm telemetry costs about 3-4 uS per motor per line direction change. So something around 24-32 uS for the line direction switching both directions together. And the rpm-filter has 48 notch filters that get dynamically tuned at 1000 hz update frequency. So running at 8k/8k will get tight. Use 4k/4k instead, at least to start with.

First install the blheli32 test fw on your escs. Also switch off any extended startup melody since that currently interferes with bidirectional dshot. The standard startup tones will work fine though.

### DMA

The current implementation requires normal DMA to be used, not burst dma. This may or may not work with a given FC. You can simply try it out:

``set dshot_burst=off``

And test whether your quad still flies. If so proceed to the next step:

### Enabling Bidirectional Dshot

``set dshot_bidir=on``

See if your motors still spin up. If so try detach the USB cable, connect a battery and reconnect USB. Now go to the CLI and type ``status``. You should see dshot telemetry being reported. The reported rpm should be zero for each motor and there should be few errors.

### Motor Poles

The escs report erpm which needs to be converted to rpm using the number of poles (magnets) of the motors. Regular 5" motors have 14 poles and that's the default setting. Smaller motors have less poles, often 12. Count them or look up the motor specs and configure using:

``set motor_poles=14``

### Enabling new scheduler policy

Since the rpm filter works with very narrow notch filters it's imperative that the gyro loop time does not vary and is exactly as high as specified. This used to require low loop rates and overclocking. A scheduler change has now been added, which allows consistent gyro rates even at higher loop rates. We recommend enabling it:

``set scheduler_optimize_rate=on``

### Verifying consistent loop time

**Important:**

After enabling all of the above features double check that your loop rate is consistent. If not select a lower loop rate. Remember that unlike effective filtering loop time has very minimal effect on [flight performance](https://github.com/betaflight/betaflight/issues/7327).

To do so enter ``status`` in the CLI and check that the gyro rate matches what you have specified.

### Supported FCs

Verified:

* OMNIBUSF4SD target (Joe)
* Banggood 20mm NOXE FC (uses NOX target, n channels reassigned breaks LED support). (Joe)
* Kakutef4v2 (bizmar)
* Airbot f4 nano v6 ( OmnibusF4FW target ) (skonk)
* MATEK405
* CLRacingF4 (joe, requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch)
* Revolt (fujin, requires [dshot_bidir_newtargets](https://github.com/joelucid/betaflight/tree/dshot_bidir_newtargets) branch)

Very likely to work (please let us know if they really do work):

* OMNIBUSF4

Please add additional verified configurations here.

### References

[Bidirectional Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[Rpm Filter PR](https://github.com/betaflight/betaflight/pull/7271)

A test version of blheli32 with erpm telemetry on signal line support is now [available](https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM/BLHeli_32%20Test%20code%20Rev32.6.1%20hex%20files)