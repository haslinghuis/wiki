##Contents

1. [How do I install Betaflight ?](#how-do-i-install-betaflight-)
1. [What is Air Mode ?](#what-is-air-mode-)
1. [How do I enable Air Mode ?](#how-do-I-enable-air-mode-)
1. [What is Acro Plus ?](#what-is-acro-plus-)
1. [What is 2khz mode ?](#what-is-2khz-mode)
1. [How do I activate 2khz mode ?](#how-do-i-activate-2khz-mode)
1. [How should I tune my Copter ?](#how-should-i-tune-my-copter-)
1. [What Flight Controllers are recommended to get the best out of BetaFlight ?](#what-flight-controllers-are-recommended-to-get-the-best-out-of-betaflight-)
1. [What are the differences between LuxFloat and Rewrite PID Controllers ?](#what-are-the-differences-between-luxfloat-and-rewrite-pid-controllers-)
1. [Is there a good resource for learning how to tune using Black Box? I'm still not sure I know what I'm looking for in the Black Box logs?](#is-there-a good-resource-for-learning-how-to-tune-using-Black-Box?-I'm-still-not-sure-I-know-what-I'm-looking-for-in-the-Black-Box-logs-)

***

##How do I install Betaflight ?

There is a step-by-step guide on how to flash the flight controller with Betaflight here: http://quadquestions.com/blog/2015/12/25/betaflight_flashing/

##What is Air Mode ?

Some users were mailing Boris about the fact their radios couldn't be configured to have Idle up switch and asking him to implement something similar in the software. Boris initially thought that this could simply just be achieved with activating the "Iterm" from zero throttle together with P and D which were already done with "pid_at_min_throttle" feature. Somehow this wasn't giving the satisfying results. It still felt weak and unresponsive. Boris was trying to wrap his head around why this was the case ! We got our P, I and D on the ground....so why isn't fully stabilizing? 

After some readings in other open source projects and some of the older discussions, he realized that the key for this was in the mixer logic as someone already had a proof of concept code to improve it, which is pretty much scaling the PID's to our throttle level and stopping the stabilisation when one motor reaches min throttle. Now Boris understood why folks always preferred this Idle up switch as it was automatically gaining a little bit more stabilisation. But this is just a workaround where you loose some throttle below! The current mixer logic sounds reasonable as the early developers were always considering the low throttle values as a NON flying situation. Guess what? In 2015 we fly a lot with 0 or low throttle and especially in the mini quad scene! This has to be changed! The real answer lies in smarter mixer approach where the calculated PID output would always consider the maximum available motor output range to be able to get the desired correction.

With AIR mode the copter will always think it's in the "AIR" and will always try to correct as fast as possible and never become weak. We of course need this stabilisation once in AIR! This has it's consequences for our ground situations which you have to be aware of. With Air mode it would mean that the motors could be spooling up after arming, but there is some protection built for that. When you arm and keep throttle stick low (below min check) it will know it is on the ground and the motors will not spool up. Once you move your throttle to higher position for more than 1 second and pitch and roll are not centered anymore it will fully activate the stabilisation with 0 throttle! So you have to be aware that if you would land very quickly after first take off that the motors now are able to spool up as the copter thinks its flying and has max ability to correct. Dont worry you can disarm now or you can keep throttle low with roll + pitch stick centered and it will still spool down or at least it will not spool up anymore.


The feature might still be optimized based on experiences of the Beta Testers, but is looking good already.

Here is some visual demonstration of how to use air mode and enjoy more in air:

http://www.youtube.com/watch?v=mlEJFMNWyvQ

http://www.youtube.com/watch?v=b0qVUa4AeDQ

##How do I enable Air Mode ? 

One method is to use a 3 way switch as follows:

Pos 1: Disarm (motors do not spin)

Pos 2: Arm (motors start spinning at **min_throttle** value)

Pos 3: Arm + Air Mode (motors keep spinning at **min_throttle** value but use the new Air Mode Mixer)

The Motor Stop Feature will need to be disabled by entering this CLI Command:

`feature -MOTOR_STOP`

Air Mode min_throttle value recommendation: "As low as possible min_throttle where motors do not stop spinning at all times is the most recommended one. I do recommend using as high as possible range for throttle like 1000-2000." - Boris comment

TODO



##What is Acro Plus ?

1. Any value of AcroPlus above 0 causes any accumulated iTerm to be reset to zero (and kept at zero) whenever your sticks are at more than 70% of full throw. When restored to less than 70% of full stick travel, iTerm is only allowed to return to 'normal' slowly, actually at 0.1% per processor loop. ITerm therefore takes about 0.5s to return to 'normal' after a flip or roll on 2kHz targets. This improves immediate post-roll/flip stability.

2. Acro Plus changes stick responsiveness by modifying the way in which the PIDs affect the motors, more so at the extreme of stick movement. 

Individual PID values are calculated as usual, so the PID sum value (the sum of pTerm, dTerm and iTerm) is calculated exactly the same. The maximum possible allowed limit for PID sum is unchanged at 1000. 

The actual PID sum value can be thought of as the actual final value sent to drive the motors. 

Acro Plus modifies the PID sum value, essentially in linear proportion to acroPlus/100, and in square proportion to the angle of the sticks, up to the the maximum possible total PID value of 1000.

If the AcroPlus value is low, i.e. 1, there is almost no change in PID sum, regardless of stick angle. Basically the PIDs work like normal, its just that the iTerm effects described above are now fully active. As usual, stick sensitivity at 100% stick travel (i.e. maximum roll rate) is set by RC and pitch/roll rates, while centre sensitivity is set by these and the amount of applied expo.

As AcroPlus values are increased, two things happen. First, the PIDsum values that normally control the motors get progressively reduced in linear proportion to stick angle. Second, and in place of the lost PIDsum values, a simple squared multiple of stick angle goes direct to the sticks.

Lets consider the numeric outcomes of the current code - I hope I've got this right:

If acroPlus is 100, and you are at 100% stick travel, you PID calculations keep happening but are completely ignored. Output to motors is simply set to the maximum allowed value for PID sum, i.e. 1000. So if you held your stick full right like this, the two left motors would basically go full on and the two right would go to min_throttle. PID loop would not constrain or control this at all. Your quad will rotate as fast as it possibly can. i.e. basically direct motor control around full sticks.

If acroPlus is 100, and you are at 50% sticks, it's a curious combination of the two. The normal PID sum calculation is still active, but the amount driving the motors is halved, while 25% of the maximum allowed output to the motors is added. So if your PID sum calculation at some instant was 350, the amount going to the motors would be 350 + 250 = 600, or 60% of maximum possible. The '250' part is fixed by the stick angle but the PID part will vary according to usual PID processes.

If acroPlus is 100, and you are at 10% sticks, it's again a combination of the two, but at lower stick values the normal PID control mechanisms very much dominate. The drive to motors will be 90% from normal PID calculations, and only 1% of the maximum allowed output to the motors will be added to that (i.e. basically normal PID operation around center sticks).

The amount of the 'direct' motor control to stick angle is stick angle % squared. Since 0.1 * 0.1 = 0.01, 10% stick angle generates only 1% of the 'direct stick control' proportion available at full stick angle. 20% stick angle generates 4% direct control, 30% -> 9%, 40% -> 16%, 50% -> 25%. Basically exponentially greater proportion of direct vs PID control.

When AcroPlus is at a number less than 100, the 'direct control' effects are linearly proportionally reduced, though you always get the full iTerm benefit. 

For example, if you set AcroPlus to 10, you get 10% of the maximum possible AcroPlus effect. That means, at 100% stick, PID sum will only get a stick angle related contribution of 100, and the rest will be determined as 90% of the normal PID sum value. At 10% stick, the stick angle related contribution to PID sum will be only 1 (1/1000th of the maximum possible) and 99% will come from normal PID processes.

Therefore you can see at higher AcroPlus values and higher stick angles, the PIDs themselves become a bit less relevant because the motors are being more directly controlled by the angles of the sticks alone. This makes motor control more direct but also much more extreme. 

Note that near or very near center sticks, AcroPlus has markedly less effect on normal PID control outputs. Hence, AcroPlus values in the 20's will have relatively little effect on center stick sensitivity, but most likely will significantly increase full stick roll rates over and above your rate settings. 

Hence Acro Plus can be considered a form of exponential rate multiplier, outside of the normal PID mechanisms, that should, in most quads, increase roll rates at high stick angles quite significantly. The iTerm coding changes prevent iTerm windup problems that would otherwise inevitably cause loss of control or serious bounce-back at the end of such extremely high rate rolls or flips.

##What is 2khz mode?
See the "Gyro based loop implementation" description on the Wiki Home page.
2kHz mode is simply a faster Gyro based loop that runs at an update rate of 2000 times a second or every 500usec.

Here is a great analogy:
You are driving a car. You run on 1khz so you are only allowed to open your eyes once per second. In this time you must not only look at the road ahead but also the wheel, speedo, RPM, radio, and so on. Now close your eyes until the next time. 
2khz let's you open your eyes and make decisions twice in the same timeframe as 1khz. 

So you are heading for a collision at 60kph. 1khz will let you adjust over that distance by a factor of one. 2khz let's you adjust by a factor of two (Yea I know, but I am explaining basics). So to make a one meter horizontal adjustment in 1K would take you 6 meters the equivalent in 2k would take you 3 meters. 
This is not gospel, just a way to explain the difference.

Have a look at this video form more information: http://www.youtube.com/watch?v=j2YtpeHGafs

##How do I activate 2khz mode?
In the CLI, make the following commands, dependent on the Flight Controller type

**For F3 boards**
```shell
set gyro_lpf = OFF
```

**For F1 boards**
```shell
set acc_hardware = 1
set baro_hardware = 1
set mag_hardware = 1
set gyro_lpf = OFF 
```

##How should I tune my Copter ?
1. It's recommended that these step be done in Acro mode even if you are usually a Level/Horizon flyer.  Example PID values shown below correspond to the Rewrite PID controller (PID controller #1)

2. Start with slightly lower than default P gains as provided by the installed BetaFlight firmware. P of 4.0 on Pitch and Roll are good starting points. Also lower the I and D gains on pitch and roll in order to tune P with minimal interference from I and D. I of 10 and D of 5 are good starting points. For yaw, it is prudent to simply decrease default P and I a bit to eliminate that axis as a source of oscillations.

3. Over a series of flights, increase P gain on Roll axis until you see oscillations when you approach full throttle and you get very rapid visible and audible shakes. Then set P term to roughly 70% of the value that caused the oscillations. 

4. Repeat step 3 for Pitch axis.

5. Test to see if the quad holds the desired roll angle and does not drift by rolling the copter to a specific angle, and then punch and drop throttle several times.  The angle you gave it relative to the horizon should not change significantly.  If the angle appears to drift, increase I gain.  Too much I gain will make the copter unresponsive and may overshoot during fast turning.

6. Repeat step 5 for Pitch axis.

7. Increase D gain ONLY to the extent that it helps reduce bounceback after flips/rolls or prop-wash oscillations after an abrupt descent. If neither is a problem, then LEAVE D LOW.  At this point the Copter should be around 80-90% tuned.

8. The defaults are usually quite good for yaw axis, and it often needs the least tuning. Because yaw inherently has less positive control (a.k.a. authority), than pitch and roll, a wider range of values are acceptable.  Relatively high P and I values and relatively low D values are the norm because of the inherent lack of authority compared to pitch and roll. A blackbox log is usually necessary to fine-tune. Most excess P oscillation comes from either roll or pitch, but if any roughness at full throttle remains, look at a blackbox log to see if yaw P starts to oscillate on full throttle. If so, decrease yaw P. 

9. Finally, refine the relationship between P and I by looking for a tendency to pull out of or push into strong turns. Can also refine P by analyzing Blackbox Logs. This may get you closer to a perfect tune.

10. Once tuning is complete in Acro mode then move onto adjusted the Level/Horizon parameters to suit your flying style if you plan to fly in angle or horizon mode.

Remember not to get too carried away trying to get the BlackBox traces to be as clean as possible. If the copter flies really well and suits your needs then just get out there and fly !

**Notes:** 

When you look for high P term oscillation in a BB log, they do not generally look like wide sweeping arcs or jagged peaks and valleys. High P term oscillations manifest first at the very top of the throttle range and look like tight sine waves.  When these show up on BB logs, they may not always be detectable by sight or sound. By the time you can see/hear them, they are super-apparent on a BB log.  That's why the recommendation for initial tuning by sight and sound is to reach the point of visible/auditory oscillation and then drop back to 70% of that value.

The undesirable flight characteristic called bounce-back oscillation occurs when you abruptly return the pitch/roll stick to center, and the copter rotation does not make a "clean" stop.  It could be the result of:

1. D that is too low

2. P that is too high

3. or even P that is too low (a low P gain can cause slow, sloppy oscillations because it's not providing enough  authority to get to the intended end-state).

**More info:**
I term is usually not active enough to cause trouble, and can usually get it roughly tuned in pretty quickly.
But the D term can vary significantly depending on many different factors, and its amplification effect means that if D term is bad, it can be very bad, and in odd and unpredictable ways, depending on how noise is presenting itself and how the P term is acting.

##What Flight Controllers are recommended to get the best out of BetaFlight ?

Here is a list of FCs compiled around the end of January 2016. The opinions regarding Pros and Cons are also shown.

| Flight Controller | Processor  | Opinion                                     |
| ----------------- | ---------- | -------------------------------------------
| **[Naze32](http://www.getfpv.com/acro-naze32-flight-controller-rev6-w-pin-headers.html)** | F1 | Available in Full and Acro flavors. Full version has the Barometer, Magnetometer and Dataflash for Black Box logs. Rev6a has SBUS Inverter while Rev5 does not. Has been a great FC, maybe getting close to end-of-life with all the new F3 boards emerging.|
| **[SPRacing F3](http://seriouslypro.com/spracingf3)**  | F3   | Hardware issues resulting in seemingly-high failure rate. Micro-connectors suck. |
| **[TBS PowerCube](http://www.team-blacksheep.com/products/prod:powercube_colibri)**     |  F3   |  Super expensive, like all TBS gear. The ESC is listed as able to run SimonK, which means it's Atmel, which means it's probably got mediocre performance.   |
| **[Dodo](http://www.rcgroups.com/forums/showthread.php?t=2439777)**  | F3   |  No complaints personally. Now that they fixed the ESC back-feeding issue, that is. So this seems to be a great option at the moment.   |
| **[MotoLab TornadoFC](http://www.rcgroups.com/forums/showthread.php?t=2473157#post32330479)** | F3 | 5v buffers on motor outputs mean no BLHeli passthrough. Other than that, awesome FC at a good price. |
| **[MotoLab Cyclone](http://dronehitech.com/motolab-cyclone-flight-controller-announced/)** | F3 |  Hasn't hit the streets yet, but looks like it ticks all the boxes. |
| **[XRacer F3](http://www.fpvmodel.com/x-racer-f303-flight-controller_g1106.html?u=8D1D164861E0E506)** | F3 |  No VBat pin? Don't know much about this one. |
| **[LUX](http://www.rcgroups.com/forums/showthread.php?t=2554204)** | F3 |  Hasn't hit the street yet, but looks good. Doesn't have a dataflash chip. |
| **[KISS](http://www.rcgroups.com/forums/showthread.php?t=2555204)** | F3 | Doesn't run Betaflight (yet) LOL. |

### Additional Information:
Overview of F1 based boards: http://www.youtube.com/watch?v=7u1PcvDosBM

##What are the differences between LuxFloat and Rewrite PID Controllers ?

According to Boris, there is literally no difference between Lux and Rewrite any more (from a flight characteristics point of view), except that they scale the numbers differently. So the actual PID gains and rates will vary between them, but the processing of gyro data is identical. The main problem with Luxfloat is that the CleanFlight Configurator GUI by default only gives you 0.1 precision, which is too big of a step for Luxfloat. It would be like trying to tune Rewrite, and only being able to use whole integers like 4.0, 5.0, 6.0. 

LuxFloat uses Floating Point maths whereas Rewrite uses Integer maths. What does this mean ? Floating Point maths needs more processing power from the Flight Controller, and so F3 (and above) CPU based FCs will have a much easier time calculating the values in the PID loop since it has a dedicated Floating Point Unit (FPU) for these calculations.
Why should this matter ? The longer the PID controller takes to process the Gyro/Accelerometer data the slower the "loop time". Faster loop times are desirable as it makes the copter more responsive to pilot commands, and also (more importantly) the ability to correct to external disturbances to the copter (like wind and propwash).

From a development point of view, LuxFloat is easier to understand since ReWrite has to emulate mathematical functions using complicated routines rather than simple commands that a dedicated FPU can handle.

People like Rewrite nowadays simply because it is easier to tune because it offers more functional precision in the gains. The days when Luxfloat and Rewrite flew differently (like before Luxfloat had error-seeking D term, or whatever) are gone.

Level mode angle control and stick sensitivity is different in Luxfloat from Rewrite in BetaFlight

In Luxfloat level/horizon modes, full max angle is reached at full stick. This means very low stick sensitivity in level/horizon modes at rates that are quite snappy in acro, especially if max_angle is set to a low value, like 45 degrees. The stick sensitivity does not change with changing rates. I personally can't fly level/horizon like this.

In rewrite, stick sensitivity is managed differently; sensitivity depends on rates and is closer to acro sensitivity. This may result in reaching max angle before the sticks reach their full travel. I personally prefer this (it was my coding hack, I think, that made it like this). It's good both for teaching and for experienced pilots.




## Is there a good resource for learning how to tune using Black Box? I'm still not sure I know what I'm looking for in the Black Box logs.

a. "I would check out Joshua Bardwells youtube channel. I haven't watched all these videos... I just picked them from his channel.

Quote:
https://www.youtube.com/watch?v=FH_m5rI6MKY
https://www.youtube.com/watch?v=hzm6H9WnCgQ
https://www.youtube.com/watch?v=Neqzeh9f-uk
https://www.youtube.com/watch?v=7UNg8fkV6zQ

Also he has at least 100 blackbox log analysis videos where he was gracious enough to help other people out. Check out those and you can learn a lot just from him reviewing peoples footage and pointing things out. There is kind of an 'art' to it so to speak ... (and goes on to mention he doesn't really use Black Box to tune) " - from powdermnky007 reply


b. More info: Joshua Bardwells's Blackbox Log Video Responses link:  http://www.rcgroups.com/forums/showthread.php?t=2484202

c. But:  "I think that even without blackbox you can get a great tune.

People don't realise that there are 2 separate things. There are rates and there are pids. The rates is something we feel even more than PIDs. There is no auto-tuning what can know what rates your brains like.
The rates are actually directly being interpreted in our brains to certain stick feel.

Good tuning just makes that feel tighter and helps removing unnecessary oscillations. But even with oscillations it doesn't mean that it will feel bad." - Boris comment

"This is the same as my experience. PIDs Parameters are one thing, and Rate Parameters are another. Take a car as an analogy.  "PID" would be like tuning the engine, so that fuel, air, timing are correct (things in the Flight Controller).  "Rates" are like tuning the steering wheel, pedals, gear (stuff you directly touch like the Transmitter Sticks) so that the 'feel' is correct. And adjustment of the Rates itself made a huge difference on how 'sensitive' the aircraft feels, especially to a noob like me"- Kuson comment

d. Battery Factor: "A while ago someone took over my pids to his quad with same setup and he said it didn't feel good. So I flew his setup and it indeed felt like PIDs were twice as low as they should be! It appeared he was using almost 2 years old (Turnigy) Nanotechs completely lost their power. Even I feel huge difference between different batteries I have." - Boris comment



##The quadcopter behaves erratic (jitters etc), after a crash,  as if P went up significant. How to fix?

"When you crash your gyro can get upset. It has always been like that even from Baseflight days.
Some gyros are more sensitive than others.
To Recalibrate Gyros: " Disarm. Perform gyro calibration (left stick down left....right stick down) and it will be fine. You will see leds blinking and it will beep. Also when plugging your lipo in your quad, * your quad should not be moved*." - Boris comment
