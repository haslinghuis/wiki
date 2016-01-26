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

If acroPlus is 100, and you are at 10% sticks, it's again a combination of the two, but at lower stick values the normal PID control mechanisms very much dominate. The drive to motors will be 90% from normal PID calculations, and only 1% of the maximum allowed output to the motors will be added to that (i.e. basically normal PID operation around centre sticks).

The amount of the 'direct' motor control to stick angle is stick angle % squared. Since 0.1 * 0.1 = 0.01, 10% stick angle generates only 1% of the 'direct stick control' proportion available at full stick angle. 20% stick angle generates 4% direct control, 30% -> 9%, 40% -> 16%, 50% -> 25%. Basically exponentially greater proportion of direct vs PID control.

When AcroPlus is at a number less than 100, the 'direct control' effects are linearly proportionally reduced, though you always get the full iTerm benefit. 

For example, if you set AcroPlus to 10, you get 10% of the maximum possible AcroPlus effect. That means, at 100% stick, PID sum will only get a stick angle related contribution of 100, and the rest will be determined as 90% of the normal PID sum value. At 10% stick, the stick angle related contribution to PID sum will be only 1 (1/1000th of the maximum possible) and 99% will come from normal PID processes.

Therefore you can see at higher AcroPlus values and higher stick angles, the PIDs themselves become a bit less relevant because the motors are being more directly controlled by the angles of the sticks alone. This makes motor control more direct but also much more extreme. 

Note that near or very near centre sticks, AcroPlus has markedly less effect on normal PID control outputs. Hence, AcroPlus values in the 20's will have relatively little effect on centre stick sensitivity, but most likely will significantly increase full stick roll rates over and above your rate settings. 

Hence Acro Plus can be considered a form of expontial rate multiplier, outside of the normal PID mechanisms, that should, in most quads, increase roll rates at high stick angles quite significantly. The iTerm coding changes prevent iTerm windup problems that would otherwise inevitably cause loss of control or serious bounce-back at the end of such extremely high rate rolls or flips.

##What is 2khz mode?

TODO 