##BetaFlight 3.0  
3.0 RC5 is out. This version has many changes in the under laying code so has its own section.  

Plan here to to list and explain only the differences from the previous 2.x version.

From Boris for those just trying 3.0:  
Just use things available in the configurator and leave the rest on default.

Betaflight has 2 different goals.
1) have super stable firmware with solid defaults to just be able to fly when you want

2) from scientific point of view it is good to keep improving and introduce new features where those who like to experiment can play with ans give valuable feedback. These are mostly burried in the cli   


##Betaflight 3.0.0-RC6

Betaflight 3.0.0-RC5 (F4 Support)

This is really a major release. The full detailled change list can be found in the the commit history.
https://github.com/betaflight/betaflight/commits/master

Short Summary of changes:

- STM32 F4 support (working blheli passthrough)
- Full IO rework
- Major target seperations. Easy implementation for new targets
- PWM code rework
- Major code cleanups and rewrites
- More configurator integration
- New Betaflight PID controller based on deg/sec. All of the future development will all happen in this single pid controller. There is still a Legacy PID controller, which is pretty much evolved rewrite. That one will stay the same.
- The new Betaflight 2DOF PID controller has some additional extra parameters for configuring. Check out the config options in the configurator. This PID controller allowes less overshoot percentage and less Derivative needed to get the same affect.
- RC Interpolation added back with multiple options. (Use Auto for automatic rx rate configuration)
- Added "diff" cli command for easier backuping of config.

RC2 - Changed defaults / cleanup ONESHOT125 feature
RC3 - Defaults based on feedback // yaw_axis added to interpolation // add additional config parameters // FIX PPM on KISS
RC4 - Defaults based on new public tests // Fixed some wrong denom defaults for SPI targets
*RC5 - Defaults based on new public tests // Fix for higher CPU due to filter reinitialisations // Add Sparky2 // Fixes for various targets *
*RC6 - Defaults // rename zero throttle stabilisation to pid_at_min_throttle // CLI cleanups *  

New 1.7.1 configurator supports some additional tuning parameters. Don't fotget to check tooltips for explainations!

The PID from 2.x versions can transfer to 3.0 as the scaling is the same, but you may expect that it should be possible to get higher PID's despite the same PID scaling due to new PID controller functionalities.


###New CLI commands
Note that most are better to set using the new BetaFlight Confug GUI.  
If a CLI command is not listed here then it is most likely not changed so look in the Wiki 'CLI command' page.
If in error, missing, etc then post a note about what is wrong in Boris' thread.
Be sure you type 'help' in the CLI to see all commands.

####diff<br />  
To see what differs from default. This is handy to learn what the config GUI does with CLI settings.    
This is also a better output for posting your CLI since then you post only setting that are different from the defaults instead of a 'dump' which outputs everything. 

####feature SUPEREXPO_RATES<br />

####set rc_interpolation = AUTO<br />
<i>[OFF, PRESET, AUTO, MANUAL]</i><br />
This feature can causing CPU to work harder to be able to run higher d setpoint weights and get cleaner motor outputs.
Set to OFF if CPU loading is too high.

####set rc_interpolation_interval = 19<br />
<i>[1..50]</i><br />

####set motor_pwm_protocol = OFF<br />
<i>[OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED]</i><br />

####set zero_throttle_stabilisation = OFF<br />
<i>[ON, OFF]</i><br />

####set airmode_activate_throttle = 1350<br />
<i>[1000..2000]</i><br />

####set yaw_rate_acceleration_limit = 50?<br />
<i>[0..200?]</i><br />
Yaw rate accel limit is the betaflight pidc replacement for yaw jump prevention. It works differently and much better. It prevents quick accelerations and decelerations of yaw axis, what were actually causing jumps.

What we do with sticks is pushing the multirotors beyond their limits, but pid controller still wants to correct that and ramps up the motors what causes jerky behavior. With accel limits the pid controller has a protection to limit the acceleration and make it smoother what also helps against iterm windups we have seen getting worse on yaw axis.
Same can also be done for roll and pitch axis what is disabled by default. It can give much smoother flight characteristics.

### The following CLI commands are Per PROFILE so can be different in each Profile.  

####set pid_tolerance_band = 0<br />
<i>[0..200]</i><br />
####set tolerance_band_min_reduction = 40<br /> ??? 
<i>[0..100]</i><br />

Reduces "hunting" effect from pid controller. 
What does the pid controller do? It hunts for error all the time. Its mainly P and D what are the quickest ones. The problem is that when error is very small like in forward flight or hover where not much error needs to be corrected the pid controller gets more "relaxed" to not keep looking for perfection. The amount of pid relaxation is determined in percentage in tolerance_band_min_reduction.
You can for example remove yaw noise on this way till certain level, but you may need to retune.  

####set pid_controller = BETAFLIGHT<br />
<i>[LEGACY, BETAFLIGHT]</i><br />

####dterm_lowpass_level = HIGH<br /> 
<i>[NORMAL, HIGH]</i><br />
####dterm_lowpass = 100<br /> 
<i>[0..500]</i><br />
####set dterm_notch_hz = 0<br /> 
<i>[0..500]</i><br />
####'dterm_notch_cutoff = 150<br />
<i>[1..500]</i><br />
####dterm_setpoint_weight = 120<br />
<i>[1..200]</i><br />


## Discussions on using the new features:

####PID control at Zero Throttle
Originally Posted by MasterZap View Post
Let me try to explain this in a clear way:

As boris say, you have three things:

    Motor Stop (nobody should use that in 2016, so forget it)
    Air mode (everyone should use that )
    Pid_at_zero_throttle


!! BORIS - CORRECT ME IF I AM WRONG !!

pid_at_zero throttle kills PID's COMPLETELY for after you just arm. The moment you spool the motors up above zero, PID's start working. Now I'm not sure if they continue to work until you disarm or not I don't know, but as you see below, that doesn't matter.

I think the motivation for this "feature" is to stop people whining about motors revving on the bench, or I dunno. For me, who run an underslung battery that barely lets the quad stand, I actually WANT pid at zero throttle, or it surely tips over when I arm it. Me, I *need* stabilization on the ground!!

Airmode also has a threshold for starting, and again, the moment you come over this threshold, it is on until you disarm. Plus, airmode overrides "Pid_at_zero_throttle". So even if "pid_at_zero_throttle" isn't sticky, airmode IS sticky, and since it overrides.... it makes pid_at_zero_throttle "effectively sticky". So the moment you've revved up, you will have full authority until you disarm.

Clear as mud?

/Z

####Notch Filters

Explained by R.A.V.

notch filter explanation

Here's an explanation for the new notch filter in 3.0 with the new PR in mind. Maybe waltr knows best where to put something from it into the wiki.

From wikipedia:

Quote:
In signal processing, a band-stop filter or band-rejection filter is a filter that passes most frequencies unaltered, but attenuates those in a specific range to very low levels. It is the opposite of a band-pass filter. A notch filter is a band-stop filter with a narrow stopband (high Q factor).

The pid loop calculates an error from the current gyro rate and a set point and will command the motors to correct this error. There is a limit to how fast motors can react and it makes no sense to try to correct anything at high frequencies above 200Hz.
This is why the lowpass filter was introduced. It will leave most frequencies below the cutoff value intact but it will already attenuate the cutoff frequency by -3dB. The attenuation will increase with higher frequency.
Unfortunately some setups are so noisy that the attenuation will not be enough and the filter cutoff has to be set very low to 70Hz or 60Hz simply to get rid of the noise above 200Hz.
This means that useful information between cutoff and 100Hz is lost. A lower cutoff also means a higher delay caused by the filter which is especially bad for dTerm and can cause more propwash.

The notch filter is an additional filter which can be enabled for gyro data and dTerm data and will remove a lot of noise from the signal before feeding it into the lowpass filter.
This way the cutoff value of the lowpass filter does not need to be lowered too much.
A notch filter with a low bandwidth in combination with a lowpass filter with high cutoff will have less delay and less noise than a lowpass filter alone with low cutoff.

By default the filter is disabled. It will be enabled when the center frequency is set.
This can be adjusted with gyro_notch_hz while the cutoff frequency at the lower side can be adjusted with gyro_notch_cutoff
For dTerm the settings are dterm_notch_hz and dterm_notch_cutoff.

Center frequency should be the mean frequency of your motors, most likely somewhere between 200Hz and 300Hz.
When setting the cutoff value you should avoid getting the filter's range below 100Hz. Keep in mind that the attenuation at this frequency is already -3dB.

My very noisy copter with mean motor frequency at 250Hz runs very well with these settings:

gyro_notch_hz = 250
gyro_notch_cutoff = 130

dterm_notch_hz = 250
dterm_notch_cutoff = 130

gyro_cutoff = 110
dterm_lpf = 0
yaw_lpf = 0

This means that the notch filter will remove noise from gyro before the lowpass filter will improve the signal further.
Because there is still some noise left in dTerm, another filter is needed. The notch filter alone is enough to remove the remaining noise in my case. It causes less latency than a lowpass filter and keeps the dTerm more in phase with pTerm.
No filtering is required for yaw for my copter with filtered gyro.

The notch filter requires some additional floating point math on each axis for gyro and dTerm so F1 targets might get slower with it enabled.
In my opinion a properly setup notch filter can also help noise free copters, especially on dTerm.


Without blackbox it will be very hard to determine the center motor frequency. I already looked into the current spectrum implementation of the viewer and will add an option to get the frequency easier. It does not need to be very precise though. I'd recommend to start with a center frequency of 200 - 250. Higher kv motors on 4s will create higher frequency noise than low kv on 3s.

Cutoff describes the lower end of the filter response and should not be too low in order to reduce latency. I don't think anyone would need it to be lower than ~130.

Here's a quick drawing of what the settings do.

Post by r.a.v. about a way to obtain data to evluate noise frequency for setting the Notch filter.
For those who want to get precise noise analysis:
You can "set debug_mode = notch" before flying. Make sure your blackbox logging rate is at least 1khz. The logging rate is based on pid-loop so 1/4 for 4k pid loop would be enough.

The debug setting will log additional data to debug[0]-debug[3]:
debug[0] is unfiltered and raw gyro data on roll axis.
debug[1] is only notch filtered gyro data on roll axis.
debug[2] is unfiltered and raw gyro data on pitch axis.
debug[3] is only notch filtered gyro data on pitch axis.

If the notch filter is disabled 0/1 and 2/3 will be identical.

A new version of blackbox-explorer will allow to view the frequency spectrum of any recorded data. 

####roll/yaw cam mix
from FieserKiller  
Note that its not active permanently in this version of BF any more. You have to configure it in modes tab. I've bound it to a switch so I can finally let my buddys fly my quad without crashing due to unfamiliar controls.   

## Discussions on using the New configurator

- measurement and error is now the d term slider thingy.
all the way to the right is error, all the way to the left is measurement. can vary.
scroll down under your PIDS.  
BF PIDC does not use the dropdown for PID_DELTA_METHOD (or the CLI variable) but the slider instead.
0 is like 2.9 measurement and 1 is like 2.9 Error.  

