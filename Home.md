![BetaFlight](https://dl.dropboxusercontent.com/u/31537757/betaflight%20logo.jpg)
# Welcome to the BetaFlight Wiki!

##Introduction
"Due to many questions about my latest Cleanflight improvements and tests here is some more central information about it. The motivation for this project is to bring the end users closer to the development. As my main focus in Cleanflight is the flight improving development. A flight test group like this is very useful.
The BetaFlight fork is from the current Cleanflight Master with possible future Cleanflight flight performance enhancements.

This project also helps contributing to other open source project like RaceFlight." - says Boris B ([lead developer](http://www.youtube.com/user/bozic1982/featured))


##Warning
By definition this firmware is Beta software and is leading-edge technology. This means that some features are experimental. While exhaustive testing is undertaken by the developers, sometimes unforeseen things can happen.

It's highly recommended that anyone using this firmware should take common sense precautions such as:

* Remove all props from the Copter before powering up
* Perform basic operational tests on the bench, such as FailSafe tests, motor spin-up tests etc
It used to be that Betaflight was mainly used by developers and experts. it's got so popular now that even newbies use it because they heard it's the best you can get. Which is true. But it does create a lot of noise. You notice it in this thread alone. It used to be a platform for dev's talking about some hard core theories/code. Now it's mainly users coming for help. And frankly, this thread is pretty overwhelming in regard of the amount of posts.

jaapp Posted this and describes BetaFlight.
 Betaflight is not newbie safe, even though many starters use it. It is not even regular user safe because of the high frequency of (non backwards compatible) changes . If the best changes in betaflight are pushed back into cleanflight, these users are offered a stable and tested version of the firmware, with great matching documentation. They can use this so they enjoy these great improvements without going through the potential hassle of dealing with experimental code and keeping up with the changes.

Meanwhile Betaflight can continue doing where it was the best in - the highly experimental features with bugs and no documentation. And the dev's don't have to focus on providing support as much as they do now.

##Motivation
The original intention of this project is only to improve testing of the current CleanFlight and new features for those who are not familiar with GitHub and compiling of own firmwares. 
After a while I realised that some things in CleanFlight are not being done on the most optimal way to give the maximum performance out of our machines.  My main focus is to prioritize acro flight behaviour and give that the main priority, but still maintain good and solid level modes. Also we do want to prevent advanced tuning and stick to only PID's adjustments. 

##Tools
Betaflight is also always being adjusted to support most current Cleanflight tools like Configurator and EzGui devices and many other MSP tools. There is no special tool needed just for betaflight.

##Tested Boards
- Naze32 rev4, rev5 and rev6 (boards like dragonfly32, flip32 are all naze32 rev4 clones and just use naze target)
- CC3D
- SpracingF3 / Dodo
- Sparky
- Motolab TornadoFC (F3) / CycloneFC (F3) / MotoF3
- Colibri Race / The cube
- SpracingF3 Mini

##Additional Remarks
- In case of using Blheli ESC software 14.1 or higher is required for reliable Betaflight support. (This remark doesnt mean that only blheli is supported. All other ESC's work fine....KISS.....Simonk etc).
On blheli esc PWM has to be set to OFF.

##Firmware Releases
Releases can be found here: https://github.com/borisbstyle/betaflight/releases or download the latest build directly from source (these are bleeding edge and may be unstable): http://andwho.sytes.net:8080/job/BorisB_BetaFlight/

##More Information

Official CleanFlight documentation: http://github.com/cleanflight/cleanflight/wiki

##Providing feedback and contributing to this project

Visit this RC Groups Forum to join the discussion: http://www.rcgroups.com/forums/showthread.php?t=2464844

Financial support to Boris.B by PayPal donation:

[![Donate](https://www.paypalobjects.com/en_US/NL/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/nl/cgi-bin/webscr?cmd=_flow&SESSION=FrZqX4LdihqTA-IsyvlDXY09Eq7UX4Ghxn9eIOQMOBHVPegu-iRC6CHOdQi&dispatch=5885d80a13c0db1f8e263663d3faee8d64ad11bbf4d2a5a1a0d303a50933f9b2)