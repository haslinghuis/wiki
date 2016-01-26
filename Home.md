# Welcome to the BetaFlight wiki!

## Current documentation

##Introduction
Due to many questions about my latest Cleanflight improvements and tests here is some more central information about it. The motivation for this project is to bring the endusers closer to the development. As my main focus in cleanflight is the flight improving development. A flight test group like this is very usefull.
The betaflight fork is from the current Cleanflight Master with possible future Cleanflight flight performance enhancements.

###Motivation
The original intention of this project is only to improve testing of the current cleanflight and new features for those who are not familliar with github and compiling of own firmwares. 
After a while I realised that some things in cleanflight are not being done on the most optimal way to give the maximum performance out of our machines.  My main focus is to prioritize acro flight behaviour and give that the main priority, but still maintain good and solid level modes. Also we do want to prevent advanced tuning and stick to only PID's adjustments. 
Some parts of proccessing are not being done efficiently like acc reading when flying acro mode, which was delaying the control loop. With this firmware disabling acc is not anymore necessary.
Also many jitter prevention have been done to maintain a steady and stable control loop and F1 targets like naze have overclocked i2c speed to be able to give same performance like standard F3 targets in terms of flight characteristics. F3 targets with i2c are also slightly overclocked to get faster communication with MPU.


Betaflight gyro filter
[![Filter](https://dl.dropboxusercontent.com/u/31537757/biquad_screen.png)](https://www.youtube.com/watch?v=Q2tSWU1MsVk)




##AIR Mode
Some users were mailing me about the fact their radios couldnt be configured to have Idle up switch and asking me to implement something simillar in the software. I initially thought that this could simply just be achived with activating the "Iterm" from zero throttle together with P and D which were already done with "pid_at_min_throttle" feature. Somehow this wasnt't giving the satisfying results. It still felt weak and unresponsive.
I was wrapping my head around the fact why this was the case. We got our P, I and D on the ground....so why isn't fully stabilizing?
After some readings in other open source projects and some of the older discussions I realized that the key for this was in the mixer logic as someone already had a proof of concept code to improve it, which is pretty much scaling the PID's to our throttle level and stopping the stabilisation when one motor reaches min throttle. Now I understood why folks always preferred this Idle up switch as it was automatically gaining a little bit more stabilisation. But this is just a workaround where you loose some throttle below! The current mixer logic sounds reasonable as the early developers were always considering the low throttle values as a NON flying situation. Guess what? In 2015 we fly a lot with 0 or low throttle and especially in the mini quad scene! This has to be changed! The real answer lies in smarter mixer approach where the calculated PID output would always consider the maximum available motor output range to be able to get te desired correction.


Few things to know about Air mode:
- With AIR mode the copter will always think it's in the "AIR" and will always try to correct as fast as possible and never become weak. We of course need this stabilisation once in AIR! This has it's consequences for our ground situations which you have to be awared of.
With Air mode it would mean that the motors could be spooling up after arming, but there is some protection built for that. When you arm and keep throttle stick low (below min check) it will know it is on the ground and the motors will not spool up. Once you move your throttle to higher position for more than 1 second and pitch and roll are not centered anymore it will fully activate the stabilisation with 0 throttle! So you have to be aware that if you would land very quickly after first take off that the motors now are able to spool up as the copter thinks its flying and has max ability to correct. Dont worry you can disarm now or you can keep throttle low with roll + pitch stick centered and it will still spool down or at least it will not spool up anymore. 

The feature might still be optimized based on experiences, but is looking good already.


Here is some visual demonstration of how to use air mode and enjoy more in air
https://www.youtube.com/watch?v=mlEJFMNWyvQ

https://www.youtube.com/watch?v=b0qVUa4AeDQ


##Gyro based loop implementation
------------------------------------------------
Gyro update is leading the loop. The loop will start after interrupt is triggered for new gyro sample. The PID controller will always be doing the calculation of the most fresh gyro value. The sampling gyro rate of 1khz will be used and that will automatically run looptimes of 1000us. This also makes the looptime setting unnecessary. There is no need for this parameter as our gyro decides when loop will run. There is no drift between gyro and control loop and your PID tune will be consistent. No aliasing should be experienced. This also helps filters to do better job in giving clean gyro traces. 

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


Download the latest build directly from source:
http://andwho.sytes.net:8080/job/BorisB_BetaFlight/