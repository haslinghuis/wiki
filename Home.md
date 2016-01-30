# Welcome to the BetaFlight wiki!

##Introduction
Due to many questions about my latest Cleanflight improvements and tests here is some more central information about it. The motivation for this project is to bring the endusers closer to the development. As my main focus in cleanflight is the flight improving development. A flight test group like this is very usefull.
The betaflight fork is from the current Cleanflight Master with possible future Cleanflight flight performance enhancements. This project also helps contributing to other open source project like raceflight.

##Motivation
The original intention of this project is only to improve testing of the current cleanflight and new features for those who are not familliar with github and compiling of own firmwares. 
After a while I realised that some things in cleanflight are not being done on the most optimal way to give the maximum performance out of our machines.  My main focus is to prioritize acro flight behaviour and give that the main priority, but still maintain good and solid level modes. Also we do want to prevent advanced tuning and stick to only PID's adjustments. 

##Quick summary of main flight performing features in BetaFlight
* Gyro sync (always the most fresh gyro data before the loop starts with minimum delay)
* Overclocked i2c bus to speed up communication with gyro
* Gyro FIR filter preconfigured forthe cleanest gyro traces to the pid controller
* Optimized D calculations and filtering for more derivative precision
* Optimized defaults with main focus on PID1 and PID2 (reduced amount of pid controllers for better overview)
* On F1 targets no need to disable acc anymore to get same performance. Gyro readings get priority when in acro mode.
* Optimized scheduling in tasks to minimize jitter to motors
* Fast PWM support in combination with Oneshot125 (This is usefull when you want fixed refresh rate for ESC's up to 4khz)
* Easy to no tuning. Stock settings for PID1 and PID2 should be very well flyable on most machines. The optimised filters do good job on feeding the cleanest gyro traces to the pid controller loop, which gives better flight experience and easier tuning.
* Cooler motors and ESC's. Better for equipment due to clean filters.
* Quaternion logic for Level modes. More precision, less drift and faster performance!
* Beeper selectable for different events
* Air mode feature
* Acro Plus system


##Tools
Betaflight is also always being adjusted to support most current Cleanflight tools like Configurator and EzGui devices and many other MSP tools. There is no special tool needed just for betaflight.

##Tested Boards
- Naze32 rev4, rev5 and rev6 (boards like dragonfly32, flip32 are all naze32 rev4 clones and just use naze target)
- CC3D
- SpracingF3 / Dodo
- Sparky
- Motolab Tornado F3
- Colibri Race / The cube

##Additional Remarks
- In case of using Blheli ESC software 14.1 or higher is required for reliable Betaflight support. (This remark doesnt mean that only blheli is supported. All other ESC's work fine....KISS.....Simonk etc).
On blheli esc PWM has to be set to OFF.

##Firmware Releases
Releases can be found here: https://github.com/borisbstyle/betaflight/releases or download the latest build directly from source (these are bleeding edge and may be unstable): http://andwho.sytes.net:8080/job/BorisB_BetaFlight/

##More Information

Official CleanFlight documentation: http://github.com/cleanflight/cleanflight/wiki

##Providing feedback and contributing to this project

Visit this RC Groups Forum to join the discussion http://www.rcgroups.com/forums/showthread.php?t=2464844

Financial support by PayPal donation: [Donate](https://www.paypal.com/nl/cgi-bin/webscr?cmd=_flow&SESSION=FrZqX4LdihqTA-IsyvlDXY09Eq7UX4Ghxn9eIOQMOBHVPegu-iRC6CHOdQi&dispatch=5885d80a13c0db1f8e263663d3faee8d64ad11bbf4d2a5a1a0d303a50933f9b2)
