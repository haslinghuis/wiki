# How should I tune my Copter ?

## Introduction

In general, to achieve a good tune, we want to use AS MUCH P as possible without introducing oscillations.  In a sense P is proportional to the amount of control you have over an axis. Less P is less positive control. More P is more positive control.  The problem is that if P gets too high, it will start to overshoot the intended end state.  This causes it to constantly overcorrect -- hence, oscillations.  High quality ESCs and faster PID loop times help with this as well by allowing the flight controller to make corrections more quickly and effectively.

I and D are only there to pick up the leftover bits of error that P can't handle:  

I looks back at accumulated error (drift) that P was unable to correct for at the time, and then adjusts for it. That's why adding I might correct when the Pitch of your copter changes unintentionally after throttle changes.  (BUT you need to try adding P FIRST. If your P is too low, then I has too much of a job to do because P has never quite done enough...)

D looks forward to see if the axis is reaching its intended value too quickly. If you give the copter a command to stop a roll very quickly, a high P value (just like we want) might tend to overshoot just a little bit and then "bounce back". IF you see a lot of this, you might want to increase D just a bit. Adding D term can also help with the small oscillations that come right after a quick change in direction or low throttle drop (prop-wash). Very important to not use too much D. USE ONLY AS MUCH AS NECESSARY BECAUSE TOO MUCH D CAUSES NOISE. NOISE THAT REACHES THE MOTORS MAY CAUSE THEM TO HEAT UP AND POSSIBLY BURN.

## Steps 

1. Make sure your motors are balanced and that your quad is as free from vibrations as practical. Trying to tune PIDs without having a clean gyro signal is like trying to build a house without having a proper foundation. [This video](https://www.youtube.com/watch?v=vjEsYei12Jw) explains an easy way to check for vibrations coming from your motors. [Adjust your lowpass filter settings](https://github.com/betaflight/betaflight/wiki/Gyro-&-Dterm-filtering-recommendations) as necessary to get a clean gyro signal.

2. It's essential that these step be done in Acro mode even if you are usually a Level/Horizon flyer.  Angle/Horizon modes have their own values that interfere with tuning.  Example PID values shown below correspond to the Rewrite PID controller (PID controller #1). Set the TPA value to 0 while performing this initial tune. TPA can be added at a later date if needed.

3. Start with slightly lower than default P gains as provided by the installed BetaFlight firmware. P of 4.0 on Pitch and Roll are good starting points. Also lower the I and D gains on pitch and roll in order to tune P with minimal interference from I and D. I of 20 and D of 5 are good starting points. For yaw, it is prudent to decrease default P by HALF and reduce I just a bit, to eliminate that axis as a source of oscillations.  Yaw will be tuned last.

4. Over a series of flights, increase P gain on Roll axis until you see oscillations when you approach full throttle and you get very rapid visible and audible shakes. Then set P term to roughly 70% of the value that caused the oscillations.

5. Repeat step 4 for Pitch axis.

6. Test to see if the quad holds the desired roll angle and does not drift by rolling the copter to a specific angle, and then punch and drop throttle several times.  The angle you gave it relative to the horizon should not change significantly.  If the angle appears to drift, increase I gain.  If you don't see drift, don't change I.  You can change the "feel" of your copter by raising or lowering I after you achieve a good tune. (I does not really affect final P and D values.

7. Repeat step 6 for Pitch axis.

8. Increase D gain on each axis ONLY to the extent that it helps reduce bounceback after flips/rolls or prop-wash oscillations after an abrupt descent. If neither is a problem, then LEAVE D LOW.  At this point the Copter should be around 80-90% tuned.  
Note: Too high of D term can cause motors to get hot. Do a short flight, 10-30 seconds, land and check motors. If you can hold your finger on the motors then they are not too hot.

9. Yaw often requires the least tuning, but it may still introduce significant oscillation if you ignore it.  Start with the Yaw P that you chopped in half in step one and verify that you do not get significant vibrations when you do a long punch-out or fast forward flight.  Start pushing up Yaw P by .5 increments until you start to see roughness through your fpv camera when in fast forward flight or punches.  Then decrease a bit.  Fine tune by looking at Yaw P term in blackbox.  It MAY be oscillating a bit, but pull up the Yaw gyro trace to see if those P oscillations actually make it to the Gyro. If the yaw gyro looks relatively smooth, you're ok. 

Note: Because yaw inherently has less positive control (a.k.a. authority), than pitch and roll, a wider range of values are acceptable.  Relatively higher P and I values and relatively low D values are the norm because of the inherent lack of authority compared to pitch and roll. A blackbox log is usually necessary to fine-tune. Most excess P oscillation comes from either roll or pitch, but if any roughness at full throttle remains, look at a blackbox log to see if yaw P starts to oscillate on full throttle. If so, decrease yaw P. 

10. Finally, refine the relationship between P and I by looking for a tendency to resist or "fall into" strong turns. Very low I values will result in an axis that drifts over time.  Low I values on an axis will allow that axis to change attitude more freely but may still hold attitude.  Higher I values on an axis will hold attitude very well, but may tend to resist movement and can add a  feeling of inertia.  Very high I values can create an overly "robotic" feeling and even oscillations.  Can also refine P by analyzing Blackbox Logs. This may get you closer to a perfect tune.

11. Once tuning is complete in Acro mode then move onto adjusted the Level/Horizon parameters to suit your flying style if you plan to fly in angle or horizon mode. (If you must.)

Remember not to get too carried away trying to get the BlackBox traces to be as clean as possible. If the copter flies really well and suits your needs then just get out there and fly !

## Other Notes

When you look for high P term oscillation in a BB log, they do not generally look like wide sweeping arcs or jagged peaks and valleys. High P term oscillations manifest first at the very top of the throttle range and look like tight sine waves.  When these show up on BB logs, they may not always be detectable by sight or sound. By the time you can see/hear them, they are super-apparent on a BB log.  That's why the recommendation for initial tuning by sight and sound is to reach the point of visible/auditory oscillation and then drop back to 70% of that value.

The undesirable flight characteristic called bounce-back oscillation occurs when you abruptly return the pitch/roll stick to center, and the copter rotation does not make a "clean" stop.  It could be the result of:

1. D that is too low

2. P that is too high

3. or even P that is too low (a low P gain can cause slow, sloppy oscillations because it's not providing enough  authority to get to the intended end-state).

I term is usually not active enough to cause trouble, and can usually get it roughly tuned in pretty quickly.
But the D term can vary significantly depending on many different factors, and its amplification effect means that if D term is bad, it can be very bad, and in odd and unpredictable ways, depending on how noise is presenting itself and how the P term is acting.

For more information see these resources:

* [Joshua Bardwell's Practical PID Tuning (Betaflight / Cleanflight)](http://www.youtube.com/playlist?list=PLwoDb7WF6c8ldO8tz0IUi9FNcJdvE2Mhe)
* FAQ page located [here](https://github.com/borisbstyle/betaflight/wiki/Frequently-Asked-Questions#is-there-a-good-resource-for-learning-how-to-tune-using-black-box-)  

### Death Rolls
The most common reason for a copter to not stop rolling (flipping) is too low of minimum throttle setting. This is basically the ESC not being able to get a motor running after commanding a motor to minimum throttle. In a BB log this shows up as a motor being Commanded to Full throttle but copter keeps rolling. First thing to try is increase the min_throttle setting (Idle % if running DSHOT protocol). 

### Yaw Twitches and Mid-Throttle Oscillations  
Another fairly common issue. Read carefully the Wiki [FAQ #56](https://github.com/borisbstyle/betaflight/wiki/Frequently-Asked-Questions#how-do-i-solve-yaw-twitches-or-mid-throttle-oscillations-)  for discussion of the issue and possible solutions.

### Additional Notes for BetaFlight version 3 (3.0 & 3.1):
1. Roll and Pitch 'P' terms can be pretty high without oscillation so increasing until oscillates then reducing may not work. 
2. There is Dterm Setpoints, weight & threshold sliders in the config GUI. These can help refine the tune. Discussion about how to adjust these is in the 3.0.x & 3.1.x Wiki pages. If increasing D term does not help bounce back then try adjust the Setpoint sliders.
3. Yaw tuning may require adjustment of lower yaw_acceleration and yaw_p_limit setting especially with high power, high kv motors.  
4. 

## Tuning the DJI Snail system  
This system needs very different PIDs and Defaults do not work causing extreme oscillations. The following video and thread covers tuning this power system (also a good video on general Tuning).   
https://www.youtube.com/watch?v=8L2v10RS6io

Thread on the DJI Snail system with lots of Tuning discussion.   
https://www.rcgroups.com/forums/showthread.php?2749559-DJI-Snail-mini-quad-propulsion-system-22-50-ESC-AND-MOTOR  

