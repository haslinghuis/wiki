# Welcome to the BetaFlight wiki!

##Introduction
Due to many questions about my latest Cleanflight improvements and tests here is some more central information about it. The motivation for this project is to bring the endusers closer to the development. As my main focus in cleanflight is the flight improving development. A flight test group like this is very usefull.
The betaflight fork is from the current Cleanflight Master with possible future Cleanflight flight performance enhancements. This project also helps contributing to other open source project like raceflight.

###Motivation
The original intention of this project is only to improve testing of the current cleanflight and new features for those who are not familliar with github and compiling of own firmwares. 
After a while I realised that some things in cleanflight are not being done on the most optimal way to give the maximum performance out of our machines.  My main focus is to prioritize acro flight behaviour and give that the main priority, but still maintain good and solid level modes. Also we do want to prevent advanced tuning and stick to only PID's adjustments. 


Betaflight gyro filter
[![Filter](https://dl.dropboxusercontent.com/u/31537757/biquad_screen.png)](https://www.youtube.com/watch?v=Q2tSWU1MsVk)


##Gyro based loop implementation
------------------------------------------------
Gyro update is leading the loop. The loop will start after interrupt is triggered for new gyro sample. The PID controller will always be doing the calculation of the most fresh gyro value. The sampling gyro rate of 1khz will be used and that will automatically run looptimes of 1000us or 500us depending of configuration and target capabilities. This also makes the looptime setting unnecessary. There is no need for this parameter as our gyro decides when loop will run. There is no drift between gyro and control loop and your PID tune will be consistent. No aliasing should be experienced. This also helps filters to do better job in giving clean gyro traces. 

![GYRO_SYNC](https://cloud.githubusercontent.com/assets/10757508/9105588/6714334c-3c19-11e5-922c-1f70d46d29ac.png)


##Tools:
Betaflight is also always being adjusted to support most current Cleanflight tools like Configurator and EzGui devices and many other MSP tools. There is no special tool needed just for betaflight.

##Tested Boards:
- Naze32 rev4, rev5 and rev6 (boards like dragonfly32, flip32 are all naze32 rev4 clones and just use naze target)
- CC3D
- SpracingF3 / Dodo
- Sparky
- Motolab Tornado F3
- Colibri Race / The cube

##Additional Remarks:
- In case of using Blheli ESC software 14.1 or higher is required for reliable Betaflight support. (This remark doesnt mean that only blheli is supported. All other ESC's work fine....KISS.....Simonk etc).
On blheli esc PWM has to be set to OFF.

##Firmware Releases:
Releases can be found here: https://github.com/borisbstyle/betaflight/releases or download the latest build directly from source (these are bleeding edge and may be unstable): http://andwho.sytes.net:8080/job/BorisB_BetaFlight/