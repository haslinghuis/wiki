##Contents

1. How do I install Betaflight ?
1. What is Air Mode ?
1. What is Acro Plus ?
1. What is 2khz mode ?
1. How do I activate 2khz mode ?
1. How should I tune my Copter ?
1. What Flight Controllers are recommended to get the best out of BetaFlight ?

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

##How do I activate 2khz mode?
In the CLI, make the following commands, dependent on the Flight Controller type

**For F3 boards**

set gyro_lpf = OFF 


**For F1 boards**

set acc_hardware = 1

set baro_hardware = 1

set mag_hardware = 1

set gyro_lpf = OFF 

##How should I tune my Copter ?
1. It's recommended that these step be done in Acro mode even if you are usually a Level/Horizon flyer.

2. Start with the default P gain as provided by the installed BetaFlight firmware and lower the I and D gains (roughly 10 and 5 respectively). These are ReWrite PID controller gains. Do this on Roll, Pitch and Yaw axis.

3. Increase P gain on Roll axis until you see oscillations (when you approach full throttle and you get very rapid visible and audible shakes). Then set P term to roughly 70% of the value that caused the oscillations. 

4. Repeat step 3 for Pitch and Yaw axis.

5. Increase I gain on Roll axis until the quad holds the desired attitude angle and does not drift. Too much I gain will make the copter unresponsive and may overshoot during fast turning.

6. Repeat step 5 for Pitch and Yaw axis.

6. Increase D gain ONLY to the extent that it helps reduce bounceback after flips or prop-wash. If neither is a problem, then LEAVE D LOW. At this point the Copter should be around 80-90% tuned.

7. Finally, refine the relationship between P and I by looking for a tendency to pull out of or push into strong turns. Can also refine P by analysing Blackbox Logs. This may get you closer to a perfect tune.

8. Once tuning is complete in Acro mode then move onto adjusted the Level/Horizon parameters to suit your flying style (if needed).

Remember not to get too carried away trying to get the BlackBox traces to be as clean as possible. If the copter flies really well and suits your needs then just get out there and fly !

**Notes:** Bounce-back oscillations could be the result of:

1. D that is too low

2. P that is too high

3. or even P that is too low (a low P gain can cause slow, sloppy oscillations because it's not providing enough  authority to get to the intended end-state).

**More info:**
I term is usually not active enough to cause trouble, and can usually get it roughly tuned in pretty quickly.
But the D term can vary significantly depending on many different factors, and its amplification effect means that if D term is bad, it can be very bad, and in odd and unpredictable ways, depending on how noise is presenting itself and how the P term is acting.

##What Flight Controllers are recommended to get the best out of BetaFlight ?

Here is a list of FCs compiled around the end of January 2016. The opinions regarding Pros and Cons are also shown.

SPRacing F3 - Hardware issues resulting in seemingly-high failure rate. Micro-connectors suck.

The Cube - Super expensive, like all TBS gear. The ESC is listed as able to run SimonK, which means it's Atmel, which means it's probably got mediocre performance.

Dodo - No complaints personally. Now that they fixed the ESC back-feeding issue, that is. Does BLHeli passthrough work on it? If so, then this one seems pretty good to me.

Moto Tornado - 5v buffers on motor outputs mean no BLHeli passthrough. Other than that, awesome FC at a good price.

Moto Cyclone - Hasn't hit the streets yet, but looks like it ticks all the boxes.

XRacer F3 - No VBat pin? Don't know much about this one.

Lux - Hasn't hit the street yet, but looks good. Doesn't have a dataflash chip, but I hate dataflash anyway.

KISS - Doesn't run Betaflight (yet) LOL.
