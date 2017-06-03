# How should I tune my Copter ?

## Introduction
#### NOTE: These Tuning instructions were written for ßF 2.x and earlier. For later versions read the additional information and discussions below. 

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
The second reason is either a Bad motor or Bad ESC or even a loose prop nut. If using BB logging then you will see a motor being Commanded to full throttle but that arm will drop (if the Accelerometer is enabled) indicating that motor is Not producing thrust. This is a Swap and try to see if it is the motor or the ESC.  
Third may be an ESC/Motor combination that just doesn't work. Not all ESC run all of the newer high power motors well. If replacing an ESC with another of the same doesn't fix the issue then try a different ESC (Brand/model).  

### Yaw Twitches and Mid-Throttle Oscillations  
Another fairly common issue. Read carefully the Wiki [FAQ #56](https://github.com/borisbstyle/betaflight/wiki/Frequently-Asked-Questions#how-do-i-solve-yaw-twitches-or-mid-throttle-oscillations-)  for discussion of the issue and possible solutions.

### Additional Notes for BetaFlight version 3 (3.0 & 3.1):
1. The Default PIDs work very well on most copters and only require slight tweaking of the PIDs.
2. Roll and Pitch 'P' terms can be pretty high without oscillation so increasing until oscillates then reducing may not work. 
3. There is Dterm Setpoints, weight & threshold sliders in the config GUI. These can help refine the tune. Discussion about how to adjust these is in the 3.0.x & 3.1.x Wiki pages. If increasing D term does not help bounce back then try adjust the Setpoint sliders.
4. Yaw tuning may require adjustment of lower yaw_accel_limit and yaw_p_limit setting especially with high power, high kv motors.  
5. Some high power systems do not fly with Default. Those require 'tuning from scratch' much like the procedures above. Check the video linked to in the DJI Snail System tuning at the bottom of this page.

### I have between 14 and 17 for D with P in the upper 50s. I can't get prop wash no matter what I try with pt1 and one or both notches disabled. Is there really any benefit to trying higher D if the quad is flying so well?  
Answer from ctzsnooze:   
D exists primarily to allow more P.

Nowadays there two relatively minor additional functions for D, but that's the original historical purpose, and still the main reason it exists.

Basically D is a 'dampener'. It pushes against any fast uncommanded changes, resisting more strongly the faster the rate of change.

If you get an un-commanded fast wobble from high P, then D can attenuate that wobble, allowing use of higher P without wobble than would be possible with no D. Classical tuning involved turning D down to near-zero, gradually increasing P until you just get wobble after sharp inputs, then bringing D up to see if you can control that wobble a bit, then see if you can add a bit more P.

Historically, P wobble happened within the control frequency range, because motors weren't so strong. It was essential to try to get P as high as possible and adding the right amount of D was essential to getting as much P as possible.

Nowadays, with more powerful motors, P alone is much more quickly able to track control inputs, because P alone can make very fast changes to the attitude of the quad. Often P can do this below its natural oscillation frequency. If the natural P oscillation frequency is high enough, the gyro filters start suppressing P feedback anyway, so D becomes less important.

Hence, with powerful motors, if you don't need to push P to the point of oscillation to get good handling, the importance of adding D becomes much less.

There is one 'modern' benefit of D that started since D became calculated from error. In betaflight this benefit only exists D weighting is above zero (ie 1 or higher in practice).

When D weighting is zero, D opposes stick inputs just as it opposes uncommanded inputs like wobbles. If you have a lot of D, the quad will become less responsive to stick inputs.

But if D weighting is above zero, a quick stick input generates a brief D spike that helps initiated the movement in the quad and avoids the tendency for D to oppose stick input. Somewhere around a weghting of 0.4-0.6 the effect is neutral, by a weighting of 1 or higher, more D makes for a twitchier quad in response to quick stick inputs, rather than opposing them. Some people like that responsiveness, others don't. A lot of D weighting and not enough P can result in a twitchy quad that is a bit floaty at the same time. But if you really like the D weighting 'feel', then more D and more D weighting gives you more of that kind of thing.

If you set the D relaxation to zero, when you return sticks to normal, D will dampen the rate of return of the quad to centre as if there was zero D weighting. This allows a quad with significant D and significant D weighing to have crisp turn initiation and smooth recovery at the end of rolls and flips. To be noticeable a lot of D is needed. Freestylers quite like this, but for racing I don't think its so good, because turn in and return behave differently. But again this effect needs D to work.

The biggest drawback of D, by far, is amplifying high frequency noise and heating up the motors with that noise. D is highly frequency sensitive. For a fixed amplitude of noise, the D signal carrying that noise back to the motors will double every time the noise frequency doubles. P doesn't do this and is much less likely to be a significant contributor to hot motors. (Blackbox logs can clearly identify which of the two is the problem, but usually it's D).

Clearly there are benefits to D and drawbacks.

My personal (racing focused) view is that we should only apply enough D to control P wobble after a classical tuning approach; no more. This will provide the most crisp handling for the quad. Using this approach you may need to filter gyros so hard because D won't be heating the motors so much; you can then put filtering more on D without much drawback, keeping the gyro/P control loop fast so it can be super effective on propwash etc.

It seems to me that more powerful the setup, the less the need for D. Typically they handle awesomely with not much P; the P resonant frequency can be high enough that the gyro filters are effective in controlling oscillation, lots of D simply isn't needed unless you want a smoothed out freestyle feel.

On the other hand, medium to low power quads need more P to feel good, for sure, and for them adding D so we can push P as high as possible is very useful; the D weighting effect then becomes quite useful.

Bottom line is that if you have a powerful quad that flies great with not much D, that's fantastic. You probably need some D to control P oscillation. But if it works fine and has no apparent P oscillation with hardly any D - ie it feels great and the motors stay cool - that's awesome. Especially for racing.  

More on D, which might not be entirely right, but it makes a ton of sense...

D's main purpose is to control P oscillations. D doesn't really have any role at all above the natural P oscillation frequency. In fact, above the natural P oscillation point, D is basically unhelpful.

If you were to do a classical tuning approach, ie where you turn P and D right down (say P to 10 and D to 2), then hover, then gradually increase P in steps until the quad oscillates on quick pitch/roll flicks, and then increase to the point where it just starts to oscillate continually (all in the absence of D), you will then be able to determine the natural P oscillation frequency.

A blackbox log would identify that frequency with precision, but just looking at it would probably be sufficient to get a decent idea. Often P oscillation is very low, as low as 5 to 10 Hz on low power quads, up to say 30-40Hz on powerful quads.

Once you know the frequency at which P oscillates, that tells you where the primary D lowpass setpoint needs to be. Probably somewhere like 50%-100% above the natural P oscillation point would be fine (and avoid phase shift issues). This could mean a D lowpass as low as 3 - 40 - 50Hz on many setups, even lower on some. It would depend on how powerful the motors are and how fast the resulting P oscillation was.

So long as the D lowpass is set above the P oscillation point - a decent way above, to avoid phase shift issues - D would still be able to properly dampen P oscillations and roll-stop wobbles etc, but low enough to have less of an effect on noise amplification at higher frequencies.

This approach suggests that there might be a logical way to determine the optimal D lowpass filter set-point value after all. I'll give it a go on the weekend.

I suggest doing a classical tuning exercise on a problematic quad? Sounds to me like you have too much P and are trying to add D in increasing amounts to control it. Classical PID theory suggests that you can't get a stable, well damped system if P is simply too high. Trying to control excessive P with more and more D doesn't work out well. As a rule of thumb, D allows pushing P 20-30% above the oscillation point. I think personally it's safer to get D to the point it helps control the oscillation, and then set P at 80% of the oscillation value. The quad may not be totally crisp like this - you could push P higher, perhaps - but it will be really stable and smooth and nicely damped. If the motors are powerful it will also be crisp and precise. You don't need a blackbox to do this; blackbox really is needed only to precisely set filters or resolve power train issues.  


### Filters:  
See the [Gyro & Filters](https://github.com/betaflight/betaflight/wiki/Gyro-&-Dterm-filtering-recommendations) Wiki Page for details and discussions on adjusting/Tuning the Filters. 

## Tuning the DJI Snail system  
This system needs very different PIDs and Defaults do not work causing extreme oscillations. The following video and thread covers tuning this power system (also a good video on general Tuning).   
https://www.youtube.com/watch?v=8L2v10RS6io

Thread on the DJI Snail system with lots of Tuning discussion.   
https://www.rcgroups.com/forums/showthread.php?2749559-DJI-Snail-mini-quad-propulsion-system-22-50-ESC-AND-MOTOR  

