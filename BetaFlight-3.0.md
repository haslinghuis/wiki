##BetaFlight 3.0  
This is really a major release. The full detailed change list can be found in the the commit history.
https://github.com/betaflight/betaflight/commits/master.  

Betaflight is a genuine Open Source project with people all over the entire world contributing to the code. It is not just me!
This 3.0 release had about 15-25 very talented developers involved working day and night for last 2 months.
I really want to thank everyone who was involved and help me and others to learn more and to make this all happen.
So many great ideas were born during last week that we can keep developing for upcoming months.   
Boris

This version has many changes in the under laying code so has its own section.  

Plan here to to list and explain only the differences from the previous 2.x versions.

From Boris for those just trying 3.0:  
Just use things available in the configurator and leave the rest on default.

Betaflight has 2 different goals.  
1) have super stable firmware with solid defaults to just be able to fly when you want

2) from scientific point of view it is good to keep improving and introduce new features where those who like to experiment can play with ans give valuable feedback. These are mostly burried in the cli   


##Betaflight 3.0.0-RC9 (F4 Support)

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
RC5 - Defaults based on new public tests // Fix for higher CPU due to filter reinitialisations // Add Sparky2 // Fixes for various targets   
RC6 - Defaults // rename zero throttle stabilisation to pid_at_min_throttle // CLI cleanups   
RC7 - Fix F4 diff/dump crashes // Fix for Sparky2 // Fix for d filter coefficients bug with higher pid denoms // Add new blackbox headers    
RC8 - Defaults (notch filter 260hz) // add "diff showdefaults" command // change some cli names // more MSP parameters // higher gpio speed for i2c gyro targets // added blackbox motor test // Improved FPV angle mix feature // Reduced PID loop busy wait // Added new Target ISHAPEDF3 // Fix PPM for Revo //  
RC9 - Support all targets (ignore int pin on pid loop)  
RC10 - Defaults // Cleanups // Drop betaflight PIDc from OPBL CC3D target (use hex for full support)  

New 1.7.6 configurator (RC10) supports some additional tuning parameters. Don't forget to check tooltips for explainations!

1.7.5 configurator for RC 8 and up supports some additional tuning parameters. Don't forget to check tooltips for explanations!
The ones who are trying a notch filter on pre RC7 releases and using separate gyro ans pid rate/denom you must upgrade to RC7 as there was a bug in coefficient calculation.  

Use 1.7.2 configurator for up to RC 7.  

New 1.7.1 configurator supports some additional tuning parameters. Don't forget to check tooltips for explainations!

The PID from 2.x versions can transfer to 3.0 as the scaling is the same, but you may expect that it should be possible to get higher PID's despite the same PID scaling due to new PID controller functionalities.


###New CLI commands
Note that most are better to set using the new BetaFlight Config GUI.  
If a CLI command is not listed here then it is most likely not changed so look in the Wiki 'CLI command' page.
If in error, missing, etc then post a note about what is wrong in Boris' thread.
Be sure you type 'help' in the CLI to see all commands.

####diff<br />  
To see what differs from default. This is handy to learn what the config GUI does with CLI settings.    
This is also a better output for posting your CLI since then you post only setting that are different from the defaults instead of a 'dump' which outputs everything. 
####diff all
shows differences in all profiles and rate profiles
####diff all commented
to see defaults

####feature SUPEREXPO_RATES or feature -SUPEREXPO_RATES<br />
Enables or disables the SuperExpo. If disabled as a Feature it can still be enable from a AUX (Mode tab) switch.

####set rc_interpolation = AUTO<br />
<i>[OFF, PRESET, AUTO, MANUAL]</i><br />
This feature can cause the CPU to work harder to be able to run higher d setpoint weights and get cleaner motor outputs. Set to OFF if CPU loading is too high.  
Note: Auto rc interpolation detects rx speed based on the reported speed by rx itself. But some receivers like also X4RS can report 9ms interval while it is actually 18ms on roll and pitch when using more channels than 8.  

####set rc_interpolation_interval = 19<br />
<i>[1..50]</i><br />

####set motor_pwm_protocol = OFF<br />
<i>[OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED]</i><br />

####set zero_throttle_stabilisation = OFF<br />
<i>[ON, OFF]</i><br />
NOTE: this is only in versions up to RC5. In RC6 and up it is changed to:
####pid_at_min_throttle = OFF<br />
<i>[ON, OFF]</i><br />
With this OFF the PIDC does NOT respond to Sticks when Throttle values in below min_check, just like in the orignal MultiWii, BaseFlight or CleanFlight.

####set airmode_activate_throttle = 1350<br />
<i>[1000..2000]</i><br />

####set yaw_rate_acceleration_limit = 50?<br />
<i>[0..200?]</i><br />
Yaw rate accel limit is the betaflight pidc replacement for yaw jump prevention. It works differently and much better. It prevents quick accelerations and decelerations of yaw axis, what were actually causing jumps.

What we do with sticks is pushing the multirotors beyond their limits, but pid controller still wants to correct that and ramps up the motors what causes jerky behavior. With accel limits the pid controller has a protection to limit the acceleration and make it smoother what also helps against iterm windups we have seen getting worse on yaw axis.
Same can also be done for roll and pitch axis what is disabled by default. It can give much smoother flight characteristics.

####set gyro_lowpass_level = HIGH<br /> 
<i>[NORMAL, HIGH]</i><br />
 Sets how aggressive/steep the cutoff is. Steeper cutoff adds more delay compared to less steep one.  
Boris states: "The gyro doesnt need a very steep cut if you ask me on a descent setup, while dterm is the one what needs more filtering".  

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

####set dterm_lowpass_level = HIGH<br /> 
<i>[NORMAL, HIGH]</i><br />
 Sets how aggressive/steep the cutoff is. Steeper cutoff adds more delay compared to less steep one.  

####set dterm_lowpass = 100<br /> 
<i>[0..500]</i><br />
####set dterm_notch_hz = 0<br />  Set to zero disables the filter.   
<i>[0..500]</i><br />
####set dterm_notch_cutoff = 150<br />
<i>[1..500]</i><br />
####set dterm_setpoint_weight = 120<br />
<i>[1..200]</i><br />

###Changes in RC8 CLI 
 New setting
####set blackbox_on_motor_test = OFF<br />
<i>[OFF,ON]</i><br />

####diff showdefaults

 New Defaults  
####set dterm_notch_hz = 260<br />
####set dterm_notch_cutoff = 160<br />
####set pid_at_min_throttle = ON


####set failsafe_procedure = DROP
<i>[AUTO-LAND,DROP]</i><br />

## Discussions on using the new features:

###Legacy PID controller
This is a rewritten MWREWRITE PID controller that uses integer math instead of Floating point math.  
Some may like this one better and it is also recommended to run faster PID loop rates on F1 processors.

###Betaflight 2DOF PID controller
This is a NEW PIDC controller.  

Post from Boris about this differences between the Legacy and 2DOF PIDC: 
http://www.rcgroups.com/forums/showpost.php?p=35460572&postcount=35822

Post from Joshua explaining 2DOF PIDC and setpoint weight:

Betaflight 3.0's new 2 degree-of-freedom PID controller is one of its most exciting features. Let's learn what the new "setpoint weight" sliders do! I also discuss the new RC interpolation feature.

Error in a PID controller can be separated into externally-induced error and error caused by moving the set-point. The 2DOF PID controller allows you to distinguish between these two types of error. In other words, it allows you to control how aggressively the PID controller responds to your stick movements.

P term setpoint weight basically controls overshoot. Higher P term setpoint weight results in sharper stick response, lower P gains, and more overshoot and oscillation. Lower P term setpoint weight results in softer stick response, higher P gains, and less overshoot and oscillation.

D term setpoint weight is harder to describe. Higher D term setpoint weight results in an overall much sharper and more precise flight feel. But it also makes the copter fly less smoothly, since it is responding exactly to every little wiggle and jiggle of the stick. Lower D term setpoint weight results in a smoother, more "organic feeling" flight experience, but also a softer and less precise one.

A very critical point to understand is that these characteristics ONLY come into play when the PID controller is responding to your stick movements. When the sticks are not moving, or are moving slowly, the effect is less pronounced or nonexistent, and the PID controller works exactly like it used to. So you can think of P, I, and D as tuning the overall response of the copter to all inputs, including external ones like wind blowing on the copter, and the setpoint weight sliders as tuning the way the copter responds specifically to stick movements. 

A video by Joshua about this:  
https://www.youtube.com/watch?v=4zncyYdAZPU

###PID control at Zero Throttle
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

###Notch Filters

notch filter explanations  

A Video from Joshua Bardwell on Notch filtering:  
https://www.youtube.com/watch?v=UQOqYOBSCc8  

A short video demonstrating with and without Notch filter from Robogenisis
https://youtu.be/ic6Np86Jsrs

Explained by R.A.V.  
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

    gyro_lowpass = 110
    dterm_lowpass = 0
    yaw_lowpass = 0

This means that the notch filter will remove noise from gyro before the lowpass filter will improve the signal further.
Because there is still some noise left in dTerm, another filter is needed. The notch filter alone is enough to remove the remaining noise in my case. It causes less latency than a lowpass filter and keeps the dTerm more in phase with pTerm.
No filtering is required for yaw for my copter with filtered gyro.

The notch filter requires some additional floating point math on each axis for gyro and dTerm so F1 targets might get slower with it enabled.
In my opinion a properly setup notch filter can also help noise free copters, especially on dTerm.


Without blackbox it will be very hard to determine the center motor frequency. I already looked into the current spectrum implementation of the viewer and will add an option to get the frequency easier. It does not need to be very precise though. I'd recommend to start with a center frequency of 200 - 250. Higher kv motors on 4s will create higher frequency noise than low kv on 3s.

Cutoff describes the lower end of the filter response and should not be too low in order to reduce latency. I don't think anyone would need it to be lower than ~130.


### How to obtain data to evaluate noise frequency for setting the Notch filter.
1. Use this setting: "set debug_mode = notch"
Make sure your blackbox logging rate is at least 1khz. The logging rate is based on pid-loop so 1/4 for 4k pid loop would be enough.
2. Fly as usual
3. Open your log in blackbox analyzer from here https://github.com/betaflight/blackbox-log-viewer
4. Add all debug options to your graph setup
5. Click on debug[0]  Note: This action of clicking on the graph section traces to the right will show the analyzer screen :D 
6. Make the graph fullscreen (next to playback options)
7. You can now see where your motor noise is most significant

Two images showing how to view the spectrum and the result of the notch filter.
![How to view Spectrum](https://cloud.githubusercontent.com/assets/17462561/17593758/43dbdefa-5fe7-11e6-9fa5-bd8e5f54e710.jpg)
![Filter result](https://cloud.githubusercontent.com/assets/17462561/17593764/45ec1a84-5fe7-11e6-80fd-861efeb56827.jpg)

The debug setting will log additional data to debug[0]-debug[3]:
* debug[0] is unfiltered and raw gyro data on roll axis.
* debug[1] is only notch filtered gyro data on roll axis.
* debug[2] is unfiltered and raw gyro data on pitch axis.
* debug[3] is only notch filtered gyro data on pitch axis.

If the notch filter is disabled 0/1 and 2/3 will be identical. Otherwise you can directly see what the filter does.
More details on phase shift for example can be found here: https://github.com/betaflight/betaflight/pull/668

Post by r.a.v.
You can view earlier BB logs (pre BF 3.0) but the analyzer won't know at which rate it was recorded. This will probably result in a correct looking spectrum but with incorrect frequency labels. Clicking on the traces on the Graph section to the right will show the analyzer screen.

Post by ctzsnooze  
All filters add delay. Doubling slope on an IIR LPF doubles delay since the same 1st order filter is simply applied twice. None currently are FIR. FIR were evaluated and not as good as simple IIR. Dterm is IIR, gyro cut was biquad (i think it still is). There is a recent post about the notch filter that linked to the GitHub page where the Notch was discussed before implementation. Diagrams there show delay for different filter combinations. Lots of thought has gone into current filter design. 

###roll/yaw cam mix
from FieserKiller  
Note that its not active permanently in this version of BF any more. You have to configure it in modes tab. I've bound it to a switch so I can finally let my buddys fly my quad without crashing due to unfamiliar controls.   

## Discussions on using the New configurator

####There are ? marks next to many of the setting Fields. Mouse Over these for a short Explanation of what they do.  

- measurement and error is now the d term slider thingy.
all the way to the right is error, all the way to the left is measurement. can vary.
scroll down under your PIDS.  
BF PIDC does not use the dropdown for PID_DELTA_METHOD (or the CLI variable) but the slider instead.
0 is like 2.9 measurement and 1 is like 2.9 Error.  
See the 2DOF PIDC details above.

- Also my blackbox logging rate is off may be a bug in running 4k/2k and on blackbox it's saying 1k is 50% when it's really 25%
Not a bug. BB Rate is a percentage of the PID loop speed, since that's where the important data comes from.  
