##Contents
1. [I'm a Neewbe, how do I start ?](#I'm-a-neewbe,-how-do-i-start-)
1. [How do I install Betaflight ?](#how-do-i-install-betaflight-)
1. [Why wont my FC board arm after upgrading the firmware?](#Why-wont-my-fc-board-arm-after-upgrading-the-firmware-)
1. [What is Air Mode ?](#what-is-air-mode-)
1. [How do I enable Air Mode ?](#how-do-i-enable-air-mode-)
1. [What is Acro Plus ?](#what-is-acro-plus-)
1. [What is 2khz mode ?](#what-is-2khz-mode-)
1. [How do I activate 2khz mode ?](#how-do-i-activate-2khz-mode-)
1. [How should I tune my Copter ?](#how-should-i-tune-my-copter-)
1. [What Flight Controllers are recommended to get the best out of BetaFlight ?](#what-flight-controllers-are-recommended-to-get-the-best-out-of-betaflight-)
1. [What are the differences between LuxFloat and Rewrite PID Controllers ?](#what-are-the-differences-between-luxfloat-and-rewrite-pid-controllers-)
1. [Is there a good resource for learning how to tune using Black Box? I'm still not sure I know what I'm looking for in the Black Box logs?](#is-there-a-good-resource-for-learning-how-to-tune-using-black-box-)
1. [The quadcopter behaves erratic (jitters etc), after a crash, as if P went up significant. How to fix?](#the-quadcopter-behaves-erratic-)
1. [How does yaw_jump_prevention_limit work ?](#how-does-yaw_jump_prevention_limit-work-)
1. [How should I configure the FailSafe system ?](#how-should-i-configure-the-failsafe-system-)
1. [What is the best practice for configuring the Throttle end points ?](#what-is-the-best-practice-for-configuring-the-throttle-end-points-)
1. [How do I configure BLHeli ESCs via BetaFlight ?](#how-do-i-configure-blheli-escs-via-betaflight-)
1. [Why does my copter flip when trying to takeoff?](#why-does-my-copter-flip-when-trying-to-takeoff-)
1. [Will the PIDs change significantly when switching from two-blades to tri-blades?](#will-the-pids-change-significantly-when-switching-from-two-blades-to-tri-blades-)
1. [Why do I have issues flashing my new F3 Flight Controller?](#why-do-i-have-issues-flashing-my-new-f3-flight-controller?-)
1. [Will Betaflight code be merged back into Cleanflight?](#will-betaflight-code-be-merged-back-into-cleanflight-)
1. [When I update to the latest version of BetaFlight do I need to recalibrate my ESCs?](#when-i-update-to-the-latest-version-of-BetaFlight-do-I-need-to-recalibrate-my-escs-)

***
##I'm a Neewbe, how do I start ?
A little history. This all started with OpenSource MultiWii code based on Arduino 8-bit boards. When the 32-bit STM32 processors become available the MutliWii code was ported to the STM32 and was called BaseFlight. Due to politics others forked the BaseFlight code to CleanFlight. More recently Boris decided that he could possibly make improvements on the way the PID control loop works and forked an Experimental version as BetaFlight.
Therefore documentation on ßF and CF tends to only show what is new or changed and the documentation of previous Firmware must be read.
Start with the **[MultiWii Wiki](http://www.multiwii.com/wiki/?title=Main_Page)**, then the **[Naze32 Manual](http://www.abusemark.com/downloads/naze32_rev2.pdf)**, the CF docs in Github an finally the ßF Github docs and this Wiki.

##How do I install Betaflight ?

There is a step-by-step guide on how to flash the flight controller with Betaflight here: http://quadquestions.com/blog/2015/12/25/betaflight_flashing/

##Why wont my FC board arm after upgrading the firmware?
Check the following:
* Perform a full chip erase while flashing the firmware.
* You can't arm the FC while in the CLI. The status light flashes rapidly.
* Try calibrating the accelerometer.

There is a new task scheduler present in firmware versions greater than 2.2.0 If upgrading from a version prior to this, then check to see if the FC status light is flashing. If it is then this indicates that there is not enough processing time to complete all the features that have been enabled.
 
In the CLI type the _tasks_ command and check the results:
 
    # tasks
    Task list:
    0 - SYSTEM, max = 10 us, avg = 0 us, total = 2 ms
    1 - GYRO/PID, max = 934 us, avg = 667 us, total = 26004 ms
    2 - ACCEL, max = 153 us, avg = 122 us, total = 974 ms
    3 - SERIAL, max = 67 us, avg = 2 us, total = 12 ms
    4 - BEEPER, max = 8 us, avg = 0 us, total = 3 ms
    5 - BATTERY, max = 40173 us, avg = 1 us, total = 47 ms
    6 - RX, max = 180 us, avg = 130 us, total = 483 ms
    7 - COMPASS, max = 156 us, avg = 125 us, total = 41 ms
    8 - BARO, max = 137 us, avg = 106 us, total = 273 ms
    10 - ALTITUDE, max = 264 us, avg = 152 us, total = 165 ms
    11 - DISPLAY, max = 130302 us, avg = 26263 us, total = 5115 ms

This shows that the copter has Display, Magnetometer, Barometer & Accelerometer systems enabled.
Try disabling **each one in turn** until the copter will arm.

The list of commands to achieve this are:

    feature -DISPLAY
    set mag_hardware = 1
    set baro_hardware = 1
    set acc_hardware = 1

Disabling the Accelerometer will force the copter into Acro mode (no self-leveling in Level and Horizon modes).

**Important:** Remember to save the CLI settings and exit the CLI (otherwise the board will not arm!)

Do not forget to check the Basics. 
Use the Receiver Tab and check that each stick moves the correct channel slider and the slider moves in the correct direction. If the wrong channel slider moves, then check the channel MAP (eg AETR instead of TAER).
Also check that the stick End Point values are still correct. For more information take a look at the Question called "What is the best practice for configuring the Throttle end points".

Is the Accelerometer Calibrated? Needs to be done once to allow arming.

To determine if the ACC or other sensor enabled is causing problems use the "status" command in the CLI. The "System load" must be less than 1.0. If greater than 1.0 then the processor has too many things to do.
      # status
      System Uptime: 10 seconds, Voltage: 0 * 0.1V (3S battery - OK), System load: 0.09
      CPU Clock=72MHz, GYRO=MPU6050, ACC=MPU6050.n, BARO=BMP280
      Cycle Time: 493, I2C Errors: 0, config size: 2020

##What is Air Mode ?

Some users were mailing Boris about the fact their radios couldn't be configured to have Idle up switch and asking him to implement something similar in the software. Boris initially thought that this could simply just be achieved with activating the "Iterm" from zero throttle together with P and D which were already done with "pid_at_min_throttle" feature. Somehow this wasn't giving the satisfying results. It still felt weak and unresponsive. Boris was trying to wrap his head around why this was the case ! We got our P, I and D on the ground....so why isn't fully stabilizing? 

After some readings in other open source projects and some of the older discussions, he realized that the key for this was in the mixer logic as someone already had a proof of concept code to improve it, which is pretty much scaling the PID's to our throttle level and stopping the stabilisation when one motor reaches min throttle. Now Boris understood why folks always preferred this Idle up switch as it was automatically gaining a little bit more stabilisation. But this is just a workaround where you loose some throttle below! The current mixer logic sounds reasonable as the early developers were always considering the low throttle values as a NON flying situation. Guess what? In 2015 we fly a lot with 0 or low throttle and especially in the mini quad scene! This has to be changed! The real answer lies in smarter mixer approach where the calculated PID output would always consider the maximum available motor output range to be able to get the desired correction.

With AIR mode the copter will always think it's in the "AIR" and will always try to correct as fast as possible and never become weak. We of course need this stabilisation once in AIR! This has it's consequences for our ground situations which you have to be aware of. With Air mode it would mean that the motors could be spooling up after arming, but there is some protection built for that. When you arm and keep throttle stick low (below min check) it will know it is on the ground and the motors will not spool up. Once you move your throttle to higher position for more than 1 second and pitch and roll are not centered anymore it will fully activate the stabilisation with 0 throttle! So you have to be aware that if you would land very quickly after first take off that the motors now are able to spool up as the copter thinks its flying and has max ability to correct. Dont worry you can disarm now or you can keep throttle low with roll + pitch stick centered and it will still spool down or at least it will not spool up anymore.


The feature might still be optimized based on experiences of the Beta Testers, but is looking good already.

### Visual demonstrations of how to use air mode and enjoy more in air

http://www.youtube.com/watch?v=mlEJFMNWyvQ

http://www.youtube.com/watch?v=b0qVUa4AeDQ

### Black Box analysis video of Air Mode

Part 1: http://www.youtube.com/watch?v=PP_De47io18

Part 2: http://www.youtube.com/watch?v=goYT3PcA-dE

Part 3: http://www.youtube.com/watch?v=z0ZUsdUD9iw

##How do I enable Air Mode ? 

One method is to use a 3 way switch as follows:

Pos 1: Disarm (motors do not spin)

Pos 2: Arm (motors start spinning at **min_throttle** value)

Pos 3: Arm + Air Mode (motors keep spinning at **min_throttle** value but use the new Air Mode Mixer)

The Motor Stop Feature will need to be disabled by entering this CLI Command:

`feature -MOTOR_STOP`

Air Mode min_throttle value recommendation: "As low as possible min_throttle where motors do not stop spinning at all times is the most recommended one. I do recommend using as high as possible range for throttle like 1000-2000." - Boris comment

**You do want min_throttle as low as possible, but a good rule of thumb is to find the throttle value (in the motors tab) at which all 4 motors spin reliably and without twitches. Then add 10 to that number and set that as min_throttle. For smaller motors (1306 3100kv motors for example), you may need to add 15 or 20 because they have less torque at very low throttle values.  As you do a low throttle flip or drop, you DON'T want the air resistance on your props to overcome the power provided at min throttle and cause the motor to bog down and stop.

From a Post by teracis:
To get airmode working all you need to do is go to the modes tab in configurator and set it to activate the same way you would with an arming switch. This is something you will need to learn so check out a tutorial rather than one of us spelling it out for you here, it will be quicker.
I suggest arming on a switch, if you want to stick arm you do so at your own peril.

If you want Airmode on permanently, tick the box and then drag the slider so it covers all the way from 1000 to 2000 and it will be on permanently. 

**Using a 3-position switch, the flight procedure could be:**

1. Connect Battery
2. Arm motors (motors start spinning)
3. Enable AirMode (no 'I' windup on ground) 
4. Lift off & fly around (motors will never stop in flight even at lowest throttle)
5. Just prior to landing, disable Air Mode (optional)
6. Land and disarm motors
Posted by BorisB: 
Guys just a quick note.

There are some people saying or complaining about their minimum throttle in airmode.

Your min_check determines your lowest possible throttle value out of your TX! The lower your min_check is configured the lower throttle you can get out of your quad in air mode.

If your min_check is set to 1100 and your TX goes down to 1000 that would mean that it is already giving some throttle. I use min_check a bit higher than 1000. I believe something like 1015 or 1020 

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

1KHz mode equals a LoopTime of 1000uSec

2KHz mode equals a LoopTime of 500uSec

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

##Limitations of 2khz mode?
Note that there is a restriction on the number of available AUX channels in 2KHz mode.

**For F3 boards**
6 AUX channels are available

**For F1 boards**
4 AUX channels are available



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

| Flight Controller | Processor | 2KHz mode | Ports   | Opinion                                     |
| ----------------- | --------- |---------- | --------| --------------------------------------------|
| **[Naze32](http://www.getfpv.com/acro-naze32-flight-controller-rev6-w-pin-headers.html)** | F1 | Y (disable Accelerometer, Barometer, Magnetometer) | CP21xx on UART1 |Available in Full and Acro flavors. Full version has the Barometer, Magnetometer and Dataflash for Black Box logs. Rev6a has SBUS Inverter while Rev5 does not. Has been a great FC, maybe getting close to end-of-life with all the new F3 boards emerging. Rev6 has MPU6500 which is not ideal. Many users are experiencing issues with the rev6 - NOT RECOMMENDED.|
| **[SPRacing F3](http://seriouslypro.com/spracingf3)**  | F3  | Y |  |Hardware issues resulting in seemingly-high failure rate. Micro-connectors suck. |
| **[TBS PowerCube](http://www.team-blacksheep.com/products/prod:powercube_colibri)**     |  F3  | Y |  | Super expensive, like all TBS gear. The ESC is listed as able to run SimonK, which means it's Atmel, which means it's probably got mediocre performance.   |
| **[Dodo](http://www.rcgroups.com/forums/showthread.php?t=2439777)**  | F3 | Y | CP21xx on UART1 |No complaints personally. Now that they fixed the ESC back-feeding issue, that is. So this seems to be a great option at the moment. Use SPRACING hex if version 3.   |
| **[MotoLab TornadoFC](http://www.rcgroups.com/forums/showthread.php?t=2473157#post32330479)** | F3 | Y | VCP USB, UARTs 1,2,3 | 5v buffers on motor outputs mean no BLHeli passthrough. Uses the STM's Virtual Com Port which requires special procedures. Other than that, awesome FC at a good price. **[Detailed instructions](http://www.rcgroups.com/forums/showthread.php?t=2537379)**|
| **[MotoLab Cyclone](http://dronehitech.com/motolab-cyclone-flight-controller-announced/)** | F3 | Y |  VCP USB, UARTs 1,2,3 |Built in 5V switching regulator. Bi-directional ESC pins for HLBeli pass-through. Looks like it ticks all the boxes. |
| **[XRacer F3](http://www.fpvmodel.com/x-racer-f303-flight-controller_g1106.html?u=8D1D164861E0E506)** | F3 | Y |  |No VBat pin? Don't know much about this one. |
| **[LUX](http://www.rcgroups.com/forums/showthread.php?t=2554204)** | F3 | Y |  VCP USB, UARTs 1,2,3 |Looks good. Doesn't have a dataflash chip. Uses the STM's Virtual Com Port which requires special procedures. Uses MPU6500 which is not ideal.|
| **[KISS](http://www.rcgroups.com/forums/showthread.php?t=2555204)** | F3 | Y |  VCP USB| Doesn't run Betaflight (yet) LOL. |
| **[SPRacingF3Mini board](http://www.rcgroups.com/forums/showthread.php?t=2592215)** | F3 | Y | VCP USB| Now supported in 2.4.0-RC6. With SD Card Socket, Race Transponder and 5V BEC. Looks good for Racing copters. |
### Additional Information:
Roundup of F1 based boards: http://www.youtube.com/watch?v=7u1PcvDosBM

Roundup of F3 based boards: http://www.youtube.com/watch?v=StnC9Q_O1Fw

Recommended CleanFlight/BetaFlight boards: http://www.youtube.com/watch?v=SJa_LgbwwMk

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
http://www.youtube.com/watch?v=FH_m5rI6MKY

http://www.youtube.com/watch?v=hzm6H9WnCgQ

http://www.youtube.com/watch?v=Neqzeh9f-uk

http://www.youtube.com/watch?v=7UNg8fkV6zQ

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

##How does yaw_jump_prevention_limit work ?
"First you need to know the basics of a mixer function on multirotors.
Mixer gets PIDsum of all 3 axis and translates that into motor output.
There is obviously a certain power available there, which is a range of max_throttle - min_throttle for each motor.
Lets say it is typically a range of 1000 (2000-1000).

So we have 4 motors determing the behaviour of quadcopter with power range of 1000 for all 3 axis mixed up scaled to the throttle.
The yaw axis is the one what requires quite a lot of power on quadcopters but also depends of setup used. The ratio of power used for yaw to get the same rotational rate is more than one for pitch and roll.
This means that hard yaw corrections could use too much of the entire available throttle range in each motor so the roll and pitch would not have enough of it. But also the way of how yaw works can create a lot of thrust where the quad would gain height to get a desired yaw correction during yaw stops what generate the most power.

Therefore the yaw_jump_prevention_limit was introduced to give a maximum of yaw PIDsum with centered sticks. That means that during yaw correction the yaw is not able to use too much of available motor power so the roll and pitch would not be affected by much and that also the hars yaw stops would not create a lot of jump.

Lowering yaw_jump_prevention_limit will result in less motor power spilled for yaw during gate clipping for example as well.

You still have the full yaw control when using stick input.

But anyway I am still surprised that your gear suffers from jump. I would say that small...and powerfull x quads would typically not suffer from jumps." - Boris B

##How should I configure the FailSafe system ?
FailSafe is something that needs to be configured in the radio receiver and the Flight Controller.
Take a look at this overview as it describes how this should be done: http://www.youtube.com/watch?v=dikr9oDzQqc

Some additional information can be found from 6:20 onwards in this video: http://www.youtube.com/watch?v=htkw7d97bOo

##What is the best practice for configuring the Throttle end points ?
This can be a difficult and confusing concept to grasp at first. The best way to describe the correct method is by way of the following tutorial video.

Part 1: http://www.youtube.com/watch?v=WFU3VewGbbA

Part 2: http://www.youtube.com/watch?v=YNRl0OTKRGA

##How do I configure BLHeli ESCs via BetaFlight ?
If you are running BetaFlight, you can program and flash your BLHeli ESCs (that have BLHeli bootloader only!) directly through the flight controller, without disconnecting the signal wires or disassembling the copter at all.

Follow this guide to learn more: http://www.youtube.com/watch?v=YWLk4qcQcvw

PLEASE NOTE: This does not work on the following boards:

| Flight Controller | Failure Reason                                      |
| ----------------- | ----------------------------------------------------|
| Moto Tornado | Since the 5v buffers on the motor outputs are uni-directional and do not support bi-directional communication. These buffers make the motor outputs more stable, but prevent passthrough. There is no software fix for this. The only fix would be a re-design of the board to remove the buffers or change them to bi-directional buffers. |
| Naze32 **Rev6** | The Naze back-fed the ESCs from the USB port. So the ESCs would power up, see the throttle signal, initialize, and then they wouldn't go into programming mode after that. The Rev6a has fixed this issue since it was released in November 2015 |

##Why does my copter flip when trying to takeoff?

Here are some likely causes:

* Motors plugged in to the wrong FC headers.
* Custom mix is incorrect.
* Motor spinning the wrong direction.
* Props on the wrong motor.
* Flight control board mounted facing the wrong direction (e.g. yaw 90 degrees left but the board_align has not been configured to reflect this).

##Will the PIDs change significantly when switching from two-blades to tri-blades?
Some have found they need a small reduction in P gains when going from two-blade to tris.
The copter was still flyable with no changes, but some have experiences increased prop-wash oscillation.
 
##Why do I have issues flashing my new F3 Flight Controller?
Some of the new F3 boards come with a Virtual COM Port (VCP) that is used to communicate with a PC or MAC over the USB interface.
Take a look at this video that talks about flashing the Lumenier LUX board that has a VCP port:

http://www.youtube.com/watch?v=b8fMsazyxDw

Within this FAQ, check the answer to "What Flight Controllers are recommended to get the best out of BetaFlight" for more details on which FC has VCP ports.

##Will Betaflight code be merged back into Cleanflight?
Yes, it is the intention that this will happen gradually over time. Sometimes features from CleanFlight also get merged into BetaFlight too.

##When I update to the latest version of BetaFlight do I need to recalibrate my ESCs?
ESCs shouldn't need recalibration unless you changed the min/max throttle values in BetaFlight.

For more information about ESC Calibration see this video: http://www.youtube.com/watch?v=o3Mg-9M0l24