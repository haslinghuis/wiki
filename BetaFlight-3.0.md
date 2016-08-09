##BetaFlight 3.0  
3.0 is currently still in Alpha testing. This version has many changes in the under laying code so has its own section.  

Plan here to to list and explain only the differences from the previous 2.x version.

From Boris for those just trying 3.0:  
Just use things available in the configurator and leave the rest on default.

Betaflight has 2 different goals.
1) have super stable firmware with solid defaults to just be able to fly when you want

2) from scientific point of view it is good to keep improving and introduce new features where those who like to experiment can play with ans give valuable feedback. These are mostly burried in the cli   


##Betaflight 3.0.0-RC5

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

New 1.7.1 configurator supports some additional tuning parameters. Don't fotget to check tooltips for explainations!

The PID from 2.x versions can transfer to 3.0 as the scaling is the same, but you may expect that it should be possible to get higher PID's despite the same PID scaling due to new PID controller functionalities.


###New CLI commands
Note that most are better to set using the new BetaFlight Confug GUI.  

####'''set pid_tolerance_band''' = ?<br />
<i>[?..?]</i><br />
####'''set pid_tolerance_band_min_reduction''' = ?<br />
<i>[?..?]</i><br />

Reduces "hunting" effect from pid controller. 
What does the pid controller do? It hunts for error all the time. Its mainly P and D what are the quickest ones. The problem is that when error is very small like in forward flight or hover where not much error needs to be corrected the pid controller gets more "relaxed" to not keep looking for perfection. The amount of pid relaxation is determined in percentage in pid_tolerance_band_min_reduction.
You can for example remove yaw noise on this way till certain level, but you may need to retune.  

####'''diff'''<br />  
to see what differs from default. This is handy to learn what the config GUI does with CLI settings.    

