##Introduction
The purpose of this page is to provide the reader with detailed information about the inner workings of the BetaFlight firmware. This information has been collected from sources such as:

* The BetaFlight RC Groups Forum (credit will be given where possible).
* YouTube Channels that have provided relevant content.

Grab a snack and make yourself comfortable ! ![Popcorn](http://static.rcgroups.com/forums/images/smilies/popcorn.gif)

##Contents
1. [Gyro based loop implementation](#gyro-based-loop-implementation-)

##Gyro based loop implementation
Gyro update is leading the loop. The loop will start after interrupt is triggered for new gyro sample. The PID controller will always be doing the calculation of the most fresh gyro value. The sampling gyro rate of 1khz will be used and that will automatically run looptimes of 1000us or 500us depending of configuration and target capabilities. This also makes the looptime setting unnecessary. There is no need for this parameter as our gyro decides when loop will run. There is no drift between gyro and control loop and your PID tune will be consistent. No aliasing should be experienced. This also helps filters to do better job in giving clean gyro traces. 

![GYRO_SYNC](https://cloud.githubusercontent.com/assets/10757508/9105588/6714334c-3c19-11e5-922c-1f70d46d29ac.png)

##delta_from_gyro and the advantage/disadvantage of having it on or off

