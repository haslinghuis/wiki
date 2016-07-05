##Contents
1. [I'm a Neewbe, how do I start ?](#im-a-neewbe-how-do-i-start-)
1. [How do I install Betaflight ?](#how-do-i-install-betaflight-)
1. [What is the difference between Min_check Min_command and Min_throttle and stick inputs ?](#what-is-the-difference-between-min_check-min_command-and-min_throttle-and-stick-inputs-)
1. [Why wont my FC board arm after upgrading the firmware ?](#why-wont-my-fc-board-arm-after-upgrading-the-firmware-)
1. [Why is the Gyro light turned off and the 3D Model not moving ?](#why-is-the-gyro-light-turned-off-and-the-3d-model-not-moving-)
1. [What is Air Mode ?](#what-is-air-mode-)
1. [How do I enable Air Mode ?](#how-do-i-enable-air-mode-)
1. [What is Acro Plus ?](#what-is-acro-plus-)
1. [What is 2khz mode ?](#what-is-2khz-mode-)
1. [How do I activate 2khz mode ?](#how-do-i-activate-2khz-mode-)
1. [What Flight Controllers are recommended to get the best out of BetaFlight ?](#what-flight-controllers-are-recommended-to-get-the-best-out-of-betaflight-)
1. [What are the differences between LuxFloat and Rewrite PID Controllers ?](#what-are-the-differences-between-luxfloat-and-rewrite-pid-controllers-)
1. [What PIDs do and how do they do it ?](#what-pids-do-and-how-do-they-do-it-)
1. [Is there a good resource for learning how to tune using Black Box ?](#is-there-a-good-resource-for-learning-how-to-tune-using-black-box-)
1. [Why does my copter behave erratic after a crash ?](#why-does-my-copter-behave-erratic-after-a-crash-)
1. [How does yaw_jump_prevention_limit work ?](#how-does-yaw_jump_prevention_limit-work-)
1. [How should I configure the FailSafe system ?](#how-should-i-configure-the-failsafe-system-)
1. [What is the best practice for configuring the Throttle end points ?](#what-is-the-best-practice-for-configuring-the-throttle-end-points-)
1. [How do I configure BLHeli ESCs via BetaFlight ?](#how-do-i-configure-blheli-escs-via-betaflight-)
1. [Why does my copter flip when trying to takeoff ?](#why-does-my-copter-flip-when-trying-to-takeoff-)
1. [Will the PIDs change significantly when switching from two-blades to tri-blades ?](#will-the-pids-change-significantly-when-switching-from-two-blades-to-tri-blades-)
1. [Why do I have issues flashing my new F3 Flight Controller ?](#why-do-i-have-issues-flashing-my-new-f3-flight-controller-)
1. [Will Betaflight code be merged back into Cleanflight ?](#will-betaflight-code-be-merged-back-into-cleanflight-)
1. [When I update to the latest version of BetaFlight do I need to recalibrate my ESCs ?](#when-i-update-to-the-latest-version-of-betaflight-do-i-need-to-recalibrate-my-escs-)
1. [Why do my motors keep accelerating on the bench when I arm without props ?](#why-do-my-motors-keep-accelerating-on-the-bench-when-i-arm-without-props-)
1. [Why do my motors spin briefly when rebooting the Flight Controller ?](#why-do-my-motors-spin-briefly-when-rebooting-the-flight-controller-)
1. [If the accelerometer is disabled and FailSafe Activates what happens to the copter ?](#if-the-accelerometer-is-disabled-and-failsafe-activates-what-happens-to-the-copter-)
1. [Why does my Flight Controller blink/beep lots of times when powering up ?](#why-does-my-flight-controller-blinkbeep-lots-of-times-when-powering-up-)
1. [My PID D gain value is small after tuning in 2khz mode is that normal ?](#my-pid-d-gain-value-is-small-after-tuning-in-2khz-mode-is-that-normal-)
1. [Why are the accelerometer Black Box traces so bad in 2KHz mode ?](#why-are-the-accelerometer-black-box-traces-so-bad-in-2khz-mode-)
1. [How do I get vbat_pid_compensation system working ?](#how-do-i-get-vbat_pid_compensation-system-working-)
1. [With vbat_pid_compensation are there issues moving from 3S to 4S batteries ?](#with-vbat_pid_compensation-are-there-issues-moving-from-3s-to-4s-batteries-)
1. [How can I run the PID controller faster than 2kHz ?](#how-can-i-run-the-pid-controller-faster-than-2khz-)
1. [What is OneShot125, OneShot42 and MultiShot and how do these relate to max_throttle and Looptime ?](#what-is-oneshot125-oneshot42-and-multishot-and-how-do-these-relate-to-max_throttle-and-looptime-)
1. [What cycle time can I run on what board ?](#what-cycle-time-can-i-run-on-what-board-)
1. [How do I go about suggesting CF Configurator enhancements ?](#how-do-i-go-about-suggesting-cf-configurator-enhancements-)
1. [How do I lower the chance of my copter producing Magic Smoke when powering on ?](#how-do-i-lower-the-chance-of-my-copter-producing-magic-smoke-when-powering-on-)
1. [Why do we have RC Rate and also Yaw Pitch Roll Rates ?](#why-do-we-have-rc-rate-and-also-yaw-pitch-roll-rates-)
1. [Why does it matter to prevent motor jitter ?](#why-does-it-matter-to-prevent-motor-jitter-)
1. [Why when I change something using CLI board crashes ?](#why-when-i-change-something-using-cli-board-crashes-)
1. [Will MW2.3 PID controller work on default PIDS ?](#will-mw23-pid-controller-work-on-default-pids-)
1. [How do I keep and then restore my Betaflight Settings each time I upgrade ?](#how-do-i-keep-and-then-restore-my-betaflight-settings-each-time-i-upgrade-)
1. [What is yaw_jump_prevention_limit and what does it do ?](#what-is-yaw_jump_prevention_limit-and-what-does-it-do-)
1. [What is yaw_iterm_reset_degrees and what does it do ?](#what-is-yaw_iterm_reset_degrees-and-what-does-it-do)
1. [How does Super Expo work ?](#how-does-super-expo-work-)
1. [How do rates relate to pitch roll & yaw degrees/s ?](#how-do-rates-relate-to-pitch-roll--yaw-degreess-)
1. [Which Flight Controllers currently use SPI ?](#which-flight-controllers-currently-use-spi-)
1. [Which HEX target do I download and flash to my Flight Controller ?](#which-hex-target-do-i-download-and-flash-to-my-flight-controller-)
1. [How do I setup for reversed prop rotation ?](#how-do-i-setup-for-reversed-prop-rotation-)
1. [What is a recommended FC and esc setup to run at 8khz, also i see reference to 4/4 or 4/4/32 or 8/8, what are these referring to?](#what-is-a-recommended-fc-and-esc-setup-to-run-at-8khz-also-i-see-reference-to-4/4-or-4/4/32-or-8/8,-what-are-these-referring-to-)  
1. [What is the difference in PIDC Iterm in ßF versions ?](#what-is-the-difference-in-PIDC-Iterm-in-ßF-versions-)
1. [How to setup blackbox record rate with onboard dataflash ?](#how-to-setup-blackbox-record-rate-with-onboard-dataflash-)
1. [How to setup the rates and SuperExpo in ßF 2.8.1](#how-to-setup-the-rates-and-SuperExpo-in-ßF-2.8.1-)

**If your question is not listed above then please check the following pages:**

http://github.com/borisbstyle/betaflight/wiki/Betaflight-specific-CLI-commands

http://github.com/borisbstyle/betaflight/wiki/BetaFlight-Deep-Dive

***
##Im a Neewbe how do I start ?
A little history. This all started with OpenSource MultiWii code based on Arduino 8-bit boards. When the 32-bit STM32 processors become available the MutliWii code was ported to the STM32 and was called BaseFlight. Due to politics others forked the BaseFlight code to CleanFlight. More recently Boris decided that he could possibly make improvements on the way the PID control loop works and forked an Experimental version as BetaFlight.
Therefore documentation on ßF and CF tends to only show what is new or changed and the documentation of previous Firmware must be read.

Start with the following video that gives a very comprehensive guide on Betaflight and the best practice approach for it's configuration:
http://www.youtube.com/watch?v=xSzO6HP6yzs

Also take a look at the **[MultiWii Wiki](http://www.multiwii.com/wiki/?title=Main_Page)**, then the **[Naze32 Manual](http://www.abusemark.com/downloads/naze32_rev2.pdf)**, the CF docs in Github an finally the ßF Github docs and this Wiki.

Fast and easy configuration  tutorial: https://youtu.be/tlfBlgcpink

Videos on Cleanflight throttle parameter configuration (RC input verse outputs to ESCs):
http://www.rcgroups.com/forums/showpost.php?p=34144329&postcount=20469

See the next FAQ topic ("How do I install Betaflight") when you are ready.

##How do I install Betaflight ?
Start with the following video that gives a very comprehensive guide on Betaflight and the best practice approach for it's configuration:
http://www.youtube.com/watch?v=xSzO6HP6yzs

There is a step-by-step guide on how to flash the flight controller with Betaflight here: http://quadquestions.com/blog/2015/12/25/betaflight_flashing/

How to flash Betaflight on CC3D video guide:
http://www.rcgroups.com/forums/showpost.php?p=34196999&postcount=21477

There is a topic on this FAQ page called "Which HEX target do I download and flash to my Flight Controller" that will help when it comes to deciding which file to use on your Flight Controller board.

#What is the difference between Min_Check Min_command and Min_throttle and stick inputs ?
From MasterZap

min_check has nothing to do with ESC's ....

min_command is the value sent when disarmed (or when armed and motor stop is on, i.e. when we want the motors not to spin).
min_throttle is the value sent when armed (with motor stop off)

min_check is about stick command and only matters towards your actual throttle stick. It has no effect on what is sent to the ESC. 

The misunderstanding of this comes from the fact that your throttle stick doesn't even begin "working" until you are above min_check. People try explaining this with sentences like "the FC will map min_check to min_throttle", which while true, makes people believe there is this relation. There is no relation. All that is being said is "the flight controller only cares about the range above min_check up to full throttle, and will remap that range into the 0%-100% input to the flight controller, which then outputs whatever it wants to the motors"

From waltr

In general (all channels) min_check & max_check are only for Stick commands. then ONLY on throttle channel min_check is used in the code for Arming and PID controller depending on other settings (pid_at_min_throttle, AirMode, etc).
mid_rc (Note: this is incorrectly label throttle middle in the CF config GUI) is telling the FC what your Stick Center value is, typically 1500 but may be 1520 on some radios. mid_rc is NOT used on throttle channel.

The default max_throttle of 1850 comes from MultiWii and is a SAFE max value for ALL ESCs.  
Code from MW2.3 config.h file  
  
 /****************************    Motor maxthrottle    *******************************/  
 /* this is the maximum value for the ESCs at full power, this value can be increased up to 2000 */  
 #define MAXTHROTTLE 1850

DEADBAND is only removing stick center value (all channels except throttle) to eliminate stick center jitter and non-returning to exactly 1500. no more, no less. Do not use this term for anything else.

Reading the MutliWii WIKI and even the MultiWii code config.h file will help to understand what these values are. A link is in the ßF Wiki, FAQ: getting started.

In CF and ßF the expected stick end point values are set with (I don't know in what versions these came about but were not in the original port of MW to BF code):
Code:

 # rxrange  
 rxrange 0 1000 2000  
 rxrange 1 1000 2000  
 rxrange 2 1000 2000  
 rxrange 3 1000 2000  

These can be adjusted for radios that can not meet the standard values.
The FC firmware uses the mid_rc and these to calculate a stick value to hand off to the PIDC code. max_check is NOT used here.

If a channel does not get to these end points then the FC will simply not see full movement, either on one side or both. This is one reason I and others and the MW Wiki and CF docs state to adjust the radios stick end points to these defaults. The other is ensuring the stick exceed the min_check, max_check thresholds so stick commands work. 


Another explanation be joshua bardwell:
Max and min channel values are determined by the rxrange command. They default to 1000 and 2000. Max_check and min_check are used to decide if you are entering a stick command. Here is the kicker--how do you disarm the copter if yaw is active? You would have to go full deflection and the copter would yaw like crazy. In order to address this, when the throttle is below min_check, and when stick arming is used (vs. switch arming), the yaw input is disabled. If you are using motor_stop, the motors also stop running when the throttle is below min_check. Sometimes, this behavior is referred to as a deadzone at the bottom of the throttle stick travel. Many people refer to this as Deadband but causes much confusion with stick center DEADBAND CLI settings, therefore DEADZONE is prefered

You can see that there is no need for a corresponding disabling of inputs at the top of the throttle range, because you never input any stick commands that require the top of the range when you are flying. The only stick command that is input when you are flying is disarm, and that is low yaw and low throttle. So there is a dead space at the bottom of the throttle range (below min_check) but no dead space at the top of any channel range. 

##Why wont my FC board arm after upgrading the firmware ?
Check the following:
* Perform a full chip erase while flashing the firmware.
* You can't arm the FC while in the CLI. The status light flashes rapidly.
* Try calibrating the accelerometer.
* Check RX basics (see below)
* Reduce the amount of aux channels to reduce time for task RX
* If the status light flashes slowly then the CPU could be over-taxed (see below).

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
    set mag_hardware = NONE
    set baro_hardware = NONE
    set acc_hardware = NONE

Disabling the Accelerometer will force the copter into Acro mode (no self-leveling in Level and Horizon modes).

**Important:** Remember to save the CLI settings and exit the CLI (otherwise the board will not arm!)

One other method to free-up the CPU is to:
* Move from PID controller LuxFloat to MWREWRITE as the later requires less CPU power.
* Disable soft serial.

Do not forget to check the Basics. 
Use the Receiver Tab and check that each stick moves the correct channel slider and the slider moves in the correct direction. If the wrong channel slider moves, then check the channel MAP (eg AETR instead of TAER).
Also check that the stick End Point values are still correct. For more information take a look at the Question called "What is the best practice for configuring the Throttle end points".
Make sure your min throttle command is lower than min_check! If in the modes tab you see that the quad should be arming but isn't, use "set min_check" and make sure that that is greater than the lowest throttle value in the receiver tab.

Is the Accelerometer Calibrated? Needs to be done once to allow arming.

To determine if the ACC or other sensor enabled is causing problems use the "status" command in the CLI. The "System load" must be less than 100%. If greater than 100% then the processor has too many things to do.

      # status
      System Uptime: 40 seconds, Voltage: 0 * 0.1V (3S battery - OK), CPU:8%
      CPU Clock=72MHz, GYRO=MPU6050, ACC=MPU6050.n, BARO=BMP280
      Cycle Time: 491, I2C Errors: 0, config size: 1308

##Why is the Gyro light turned off and the 3D Model not moving ?
This is a side effect of the accelerometer being disabled.  When connected to the Flight Controller via USB, the 3D model in Cleanflight Configurator depends on the accelerometer to rotate properly when the multirotor is moved around.  The gyro light being off is just a glitch in the Configurator.  Neither of these are anything to worry about, it is perfectly normal.

When you change your looptime in the Configurator (or via CLI command) to a faster speed than the defaults, Betaflight will automatically disable the accelerometer on some targets to free up processing power and allow the faster looptime.

##What is Air Mode ?
Some users were mailing Boris about the fact their radios couldn't be configured to have Idle up switch and asking him to implement something similar in the software. Boris initially thought that this could simply just be achieved with activating the "Iterm" from zero throttle together with P and D which were already done with "pid_at_min_throttle" feature. Somehow this wasn't giving the satisfying results. It still felt weak and unresponsive. Boris was trying to wrap his head around why this was the case ! We got our P, I and D on the ground....so why isn't fully stabilizing? 

After some readings in other open source projects and some of the older discussions, he realized that the key for this was in the mixer logic as someone already had a proof of concept code to improve it, which is pretty much scaling the PID's to our throttle level and stopping the stabilization when one motor reaches min throttle. Now Boris understood why folks always preferred this Idle up switch as it was automatically gaining a little bit more stabilization. But this is just a workaround where you loose some throttle below! The current mixer logic sounds reasonable as the early developers were always considering the low throttle values as a NON flying situation. Guess what? In 2015 we fly a lot with 0 or low throttle and especially in the mini quad scene! This has to be changed! The real answer lies in smarter mixer approach where the calculated PID output would always consider the maximum available motor output range to be able to get the desired correction.

With AIR mode the copter will always think it's in the "AIR" and will always try to correct as fast as possible and never become weak. We of course need this stabilization once in AIR! This has it's consequences for our ground situations which you have to be aware of. With Air mode it would mean that the motors could be spooling up after arming, but there is some protection built for that. When you arm and keep throttle stick low (below min check) it will know it is on the ground and the motors will not spool up. Once you move your throttle to higher position for more than 1 second and pitch and roll are not centered anymore it will fully activate the stabilization with 0 throttle! So you have to be aware that if you would land very quickly after first take off that the motors now are able to spool up as the copter thinks its flying and has max ability to correct. Dont worry you can disarm now or you can keep throttle low with roll + pitch stick centered and it will still spool down or at least it will not spool up anymore.


The feature might still be optimized based on experiences of the Beta Testers, but is looking good already.

### Visual demonstrations of how to use air mode and enjoy more in air

http://www.youtube.com/watch?v=mlEJFMNWyvQ

http://www.youtube.com/watch?v=b0qVUa4AeDQ

### A Joshua Bardwell video on Air mode
https://www.youtube.com/watch?v=d2nRrVENEYM

### Black Box analysis video of Air Mode

Part 1: http://www.youtube.com/watch?v=PP_De47io18

Part 2: http://www.youtube.com/watch?v=goYT3PcA-dE

Part 3: http://www.youtube.com/watch?v=z0ZUsdUD9iw

##How do I enable Air Mode ? 
One method is to use a 3 way switch as follows:  
Pos 1: Disarm (motors do not spin)  
Pos 2: Arm (motors start spinning at **min_throttle** value)  
Pos 3: Arm + Air Mode (motors keep spinning at **min_throttle** value but use the new Air Mode Mixer)

The Motor Stop Feature can be enabled or disabled, and it will behave as it normally does when Air Mode is not active. However, once Air Mode is activated, Motor Stop will be overridden, and the motors will spin at min_throttle or above.

Air Mode min_throttle value recommendation: "As low as possible min_throttle where motors do not stop spinning at all times is the most recommended one. I do recommend using as high as possible range for throttle like 1000-2000." - Boris comment

**You do want min_throttle as low as possible, but a good rule of thumb is to find the throttle value (in the motors tab) at which all 4 motors spin reliably and without twitches. Then add 10 to that number and set that as min_throttle. For smaller motors (1306 3100kv motors for example), you may need to add 15 or 20 because they have less torque at very low throttle values.  As you do a low throttle flip or drop, you DON'T want the air resistance on your props to overcome the power provided at min throttle and cause the motor to bog down and stop.  
Note: some ESCs can desync on very low min_throttle values when throttle is raised from zero rapidly. If this occurs then increase the min_throttle value another 20-40sec or until the ESCs do not desync on rapid throttle increase from zero.  

From a Post by teracis:
To get airmode working all you need to do is go to the modes tab in configurator and set it to activate the same way you would with an arming switch. This is something you will need to learn so check out a tutorial rather than one of us spelling it out for you here, it will be quicker.
I suggest arming on a switch, if you want to stick arm you do so at your own peril.

If you want Airmode on permanently, tick the box and then drag the slider so it covers all the way from 1000 to 2000 and it will be on permanently. 

**Using a 3-position switch, the flight procedure could be:**

1. Connect Battery and ensure the copter DOES NO MOVE while the FC boots and does a Gyro Cal. The beeper will beep three times once the Gyro is cal'ed.  
2. Arm motors (motors start spinning)
3. Enable AirMode (no 'I' windup on ground) 
4. Lift off & fly around (motors will never stop in flight even at lowest throttle)
5. Just prior to landing, disable Air Mode (optional)
6. Land and disarm motors

There are some people saying or complaining about their minimum throttle in airmode.

Your min_check determines your lowest possible throttle value out of your TX! The lower your min_check is configured the lower throttle you can get out of your quad in air mode.

If your min_check is set to 1100 and your TX goes down to 1000 that would mean that it is already giving some throttle. I use min_check a bit higher than 1000. I believe something like 1015 or 1020 

If you have difficulty with bounce or other unwanted actions upon landing then disable the "Disarm motor regardless of throttle value". What this allows is putting the aux switch to the disarm position while flying and keeps the copter armed as long as the throttle stick value stays above min_check. Then upon landing drop the throttle stick to zero (below min_check) and the copter disarms. No bounce or other issues and no need to reach for the disarm switch upon landing.

##What is Acro Plus ?
1. Any value of AcroPlus above 0 causes any accumulated iTerm to be reset to zero (and kept at zero) whenever your sticks are at more than 70% of full throw. When restored to less than 70% of full stick travel, iTerm is only allowed to return to 'normal' slowly, actually at 0.1% per processor loop. ITerm therefore takes about 0.5s to return to 'normal' after a flip or roll on 2kHz targets. This improves immediate post-roll/flip stability.

2. AcroPlus changes stick responsiveness by modifying the way in which the PIDs affect the motors, more so at the extreme of stick movement. 

Individual PID values are calculated as usual, so the PID sum value (the sum of pTerm, dTerm and iTerm) is calculated exactly the same. The maximum possible allowed limit for PID sum is unchanged at 1000. 

The actual PID sum value can be thought of as the actual final value sent to drive the motors. 

Acro Plus modifies the PID sum value, essentially in linear proportion to acroPlus/100, and in square proportion to the angle of the sticks, up to the the maximum possible total PID value of 1000.

If the AcroPlus value is low, i.e. 1, there is almost no change in PID sum, regardless of stick angle. Basically the PIDs work like normal, its just that the iTerm effects described above are now fully active. As usual, stick sensitivity at 100% stick travel (i.e. maximum roll rate) is set by RC and pitch/roll rates, while center sensitivity is set by these and the amount of applied expo.

As AcroPlus values are increased, two things happen. First, the PIDsum values that normally control the motors get progressively reduced in linear proportion to stick angle. Second, and in place of the lost PIDsum values, a simple squared multiple of stick angle goes direct to the motors.

Lets consider the numeric outcomes of the current code - I hope I've got this right:

If AcroPlus is 100, and you are at 100% stick travel, you PID calculations keep happening but are completely ignored. Output to motors is simply set to the maximum allowed value for PID sum, i.e. 1000. So if you held your stick full right like this, the two left motors would basically go full on and the two right would go to min_throttle. PID loop would not constrain or control this at all. Your quad will rotate as fast as it possibly can. i.e. basically direct motor control around full sticks.

If AcroPlus is 100, and you are at 50% sticks, it's a curious combination of the two. The normal PID sum calculation is still active, but the amount driving the motors is halved, while 25% of the maximum allowed output to the motors is added. So if your PID sum calculation at some instant was 350, the amount going to the motors would be 350 + 250 = 600, or 60% of maximum possible. The '250' part is fixed by the stick angle but the PID part will vary according to usual PID processes.

If AcroPlus is 100, and you are at 10% sticks, it's again a combination of the two, but at lower stick values the normal PID control mechanisms very much dominate. The drive to motors will be 90% from normal PID calculations, and only 1% of the maximum allowed output to the motors will be added to that (i.e. basically normal PID operation around center sticks).

The amount of the 'direct' motor control to stick angle is stick angle % squared. Since 0.1 * 0.1 = 0.01, 10% stick angle generates only 1% of the 'direct stick control' proportion available at full stick angle. 20% stick angle generates 4% direct control, 30% -> 9%, 40% -> 16%, 50% -> 25%. Basically exponentially greater proportion of direct vs PID control.

When AcroPlus is at a number less than 100, the 'direct control' effects are linearly proportionally reduced, though you always get the full iTerm benefit. 

For example, if you set AcroPlus to 10, you get 10% of the maximum possible AcroPlus effect. That means, at 100% stick, PID sum will only get a stick angle related contribution of 100, and the rest will be determined as 90% of the normal PID sum value. At 10% stick, the stick angle related contribution to PID sum will be only 1 (1/1000th of the maximum possible) and 99% will come from normal PID processes.

Therefore you can see at higher AcroPlus values and higher stick angles, the PIDs themselves become a bit less relevant because the motors are being more directly controlled by the angles of the sticks alone. This makes motor control more direct but also much more extreme. 

Note that near or very near center sticks, AcroPlus has markedly less effect on normal PID control outputs. Hence, AcroPlus values in the 20's will have relatively little effect on center stick sensitivity, but most likely will significantly increase full stick roll rates over and above your rate settings. 

Hence Acro Plus can be considered a form of exponential rate multiplier, outside of the normal PID mechanisms, that should, in most quads, increase roll rates at high stick angles quite significantly. The iTerm coding changes prevent iTerm windup problems that would otherwise inevitably cause loss of control or serious bounce-back at the end of such extremely high rate rolls or flips.


**TODO**
Re-write explanation to include the two possible factor (CLI commands) set acro_plus_factor and set acro_plus_offset. Also discuss the changes in the different Version of ßF Firmware.

##What is 2kHz mode ?
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

##How do I activate 2kHz mode ?
For Betaflight 2.4.0 onwards you should NOT use CLI but rather set looptime to 500 in the Configuration tab of the GUI. CAUTION: Appropriate sensors will automatically be disabled on F1 boards.

For betaflight version prior to 2.4.0 can use the CLI and make the following commands, dependent on the Flight Controller type:
 
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

##Limitations of 2kHz mode
Note that there is a restriction on the number of available AUX channels in 2kHz mode (actually on any loop frequency greater than 1kHz).

**For F3 boards**

6 AUX channels are available

**For F1 boards**

4 AUX channels are available

For Betaflight 2.4.1 onwards the number of Aux channels is selectable with the set max_aux_channels (see the CLI commands section of wiki).

**Note:**
Some ESCs will not calibrate at 2kHz and faster Loop Rates, KISS ESCs have been reported to have this issue. The fix is to simply set Loop rate to 1kHz (1000usec looptime) then calibrate the ESCs and change back to the desired looptime.

##What Flight Controllers are recommended to get the best out of BetaFlight ?
Here is a list of FCs compiled around the end of January 2016. The opinions regarding Pros and Cons are also shown.

| Flight Controller | Processor/Sensor | 2KHz mode | Ports   | Opinion                                     |
| ----------------- | ---------------- |---------- | --------| --------------------------------------------|
| Naze32 rev. 5 | F1 MPU6050-I2C | Y (disable Accelerometer, Barometer, Magnetometer) |UARTs 1 and 2. UART 1 shared with USB  |Comes in acro and full. Full version has a barometer, magnetometer and dataflash for Blackbox. Uses relatively inconvenient pads for the receiver. A tried and tested board but now superseded by the rev. 6.|
| **[Naze32 rev. 6](http://www.getfpv.com/acro-naze32-flight-controller-rev6-w-pin-headers.html)** | F1 MPU6500-SPI | Y (disable Accelerometer, Barometer, Magnetometer)|UARTs 1 and 2. UART 1 shared with USB | Now even the acro version has a barometer and datafash. Uses through-hole instead of pads. USB connector moved to the right. Also has an SBus inverter. 6a fixes the issue with ESC calibration and 6b fixes the Spektrum sat issue. The F1 processor is starting to reach its limits and an F3 board is advised. Several users are reporting erratic behaviour and strange issues with this board that could be due to the use of the MPU6500 gyro that has a worse noise spec than the 6050. There is also a [compelling theory](https://www.youtube.com/watch?v=dRyOS9TvIV4) that the MPU6500 does not like high vibration environments.|
| **[SPRacing F3](http://seriouslypro.com/spracingf3)**  | F3  | Y |  |Hardware issues resulting in seemingly-high failure rate. Micro-connectors suck. |
|**Flip32/Flip32+** **[DragonFly32/DF32+](http://www.rcgroups.com/forums/showthread.php?t=2320471&highlight=dragonfly)** | F1 MPU6050-I2C | Y (disable Accelerometer, Barometer, Magnetometer) | UARTs 1 and 2. UART 1 shared with USB | Comes in acro and plus. Plusversion has a barometer, magnetometer. This is a Clone of the Nase32 but with Through Hole connector pads.
| **[TBS PowerCube](http://www.team-blacksheep.com/products/prod:powercube_colibri)**     |  F3  | Y |  | Super expensive, like all TBS gear. The ESC is listed as able to run SimonK, which means it's Atmel, which means it's probably got mediocre performance.   |
| **[Dodo](http://www.rcgroups.com/forums/showthread.php?t=2439777)**  | F3 MPU6050-I2C | Y | CP21xx on UART1 |No complaints personally. Now that they fixed the ESC back-feeding issue, that is on V3. Also has 2MB SPI Flash on board for BB logging. So this seems to be a great option at the moment. |
| **[MotoLab TornadoFC](http://www.rcgroups.com/forums/showthread.php?t=2473157#post32330479)** | F3 | Y | VCP USB, UARTs 1,2,3 | 5v buffers on motor outputs mean no BLHeli passthrough. Uses the STM's Virtual Com Port which requires special procedures. Other than that, awesome FC at a good price. **[Detailed instructions](http://www.rcgroups.com/forums/showthread.php?t=2537379)**|
| **[MotoLab Cyclone](http://dronehitech.com/motolab-cyclone-flight-controller-announced/)** | F3 MPU6000-SPI| Y |  VCP USB, UARTs 1,2,3 |Built in 5V switching regulator. Bi-directional ESC pins for HLBeli pass-through. Uses a VCP which means an external USB to Serial device must be connected to use BLHeli passthrough until passthrough over VCP is added. Does not have on board dataflash |
| **[XRacer F3](http://www.fpvmodel.com/x-racer-f303-flight-controller_g1106.html?u=8D1D164861E0E506)** | F3 MPU6050-I2C | Y |  |One of the cheapest F3 boards available. Nice design and board layout though it lacks pins for VBat and RSSI. VBat can be added by soldering a voltage divider directly to the processor. Has more dataflash than any other board. See **[here](http://intofpv.com/t-x-racer-f3-fc-adding-vbat-hack)**. v2 of the board has VBat and RSSI solder pads.
| **[LUX](http://www.rcgroups.com/forums/showthread.php?t=2554204)** | F3 MPU6500-SPI | Y |  VCP USB, UARTs 1,2,3 |Looks good. Doesn't have a dataflash chip. Uses the STM's Virtual Com Port which requires special procedures. Uses MPU6500 which is not ideal.|
| **[KISS](http://www.rcgroups.com/forums/showthread.php?t=2555204)** | F3 MPU6050-I2C | Y |  VCP USB| Doesn't run Betaflight (yet) LOL. |
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

Boris posted this in the thread about Tuning Rewrite to feel the same as Luxfloat:

I was saying that if you would take some time and work out the math that you could produce same numbers with both
Just an example to show you. You don't have to understand the code to understand this part.

Luxfloat P
Code:

        // -----calculate P component
        PTerm = RateError * P * TPA;

rewrite P
Code:

        // -----calculate P component
        PTerm = (RateError * P * TPA) / 128;

The difference above is that P gain number on rewrite is higher, but is being divided by 128 in the PTerm calculation, while luxfloat uses directly the number you entered from cli. Note that RateError number is using degrees/sec in luxfloat and in rewrite its abstraction from the original gyro output, but both can produce same PTerm when right P is selected.

So if you can find P component what can produce the same PTerm result you will get same behaviour.

Practical translation:
1.0 in luxfloat means exactly 4.0 in rewrite just for Pterm.

This same translation formula can be done on all numbers like rates, Dterm and Iterm. 

New in V2.5.4

The 'P' and 'D' Terms in Luxfloat are now shown as 4 times higher to allow better resolution when tuning. The actual PID scaling stays the same and can be seen in the CLI.

Additional comment from Boris on MW-rewrite verse Luxfloat:  
There should not be any difference between both in terms of PID's and rates. Well there is one slight difference actually, which I forgot to mention and even I forgot about it.  
rewrite still has a bit higher D range. To be exact rewrite has 2x higher delta for Dterm due to averaging summing instead of average dividing the sum. I would rather like to remove this, but dont want to cause people having to retune their rewrite. But even though with this Dterm rewrite should in theory handle bounces better....right? But that isnt the case.  

I know why. Rewrite has a Dterm deadband integrated in the integer logic, which helps keeping some noise away. But the lower numbers can cause some aliasing in dterm and some lower frequencies which aren't there may be thrown into pid controller.  
There will be some more data about this soon to confirm, but luxfloat may now become a winner certainly now where it became better tunable.  

New in ßF 2.8 and above: the PIDC names LUXFLOAT & MWREWRITE are no longer used since Boris has rewritten the code and they not longer use the same algorithm as before. The new names are FLOAT & INTEGER.
  
## What PIDs do and how do they do it ?
Here a good basic PID explaination by Bruce for those who want to learn about it. 
https://www.youtube.com/watch?v=0vqWyramGy8  

## Is there a good resource for learning how to tune using Black Box ?
a. "I would check out Joshua Bardwells youtube channel. I haven't watched all these videos... I just picked them from his channel.

Quote:
http://www.youtube.com/watch?v=FH_m5rI6MKY

http://www.youtube.com/watch?v=hzm6H9WnCgQ

http://www.youtube.com/watch?v=Neqzeh9f-uk

http://www.youtube.com/watch?v=7UNg8fkV6zQ

Also he has at least 100 blackbox log analysis videos where he was gracious enough to help other people out. Check out those and you can learn a lot just from him reviewing peoples footage and pointing things out. There is kind of an 'art' to it so to speak ... (and goes on to mention he doesn't really use Black Box to tune) " - from powdermnky007 reply


b. More info: Joshua Bardwells's Blackbox Log Video Responses link:  http://www.rcgroups.com/forums/showthread.php?t=2484202

c. But:  "I think that even without blackbox you can get a great tune.

People don't realize that there are 2 separate things. There are rates and there are pids. The rates is something we feel even more than PIDs. There is no auto-tuning what can know what rates your brains like.
The rates are actually directly being interpreted in our brains to certain stick feel.

Good tuning just makes that feel tighter and helps removing unnecessary oscillations. But even with oscillations it doesn't mean that it will feel bad." - Boris comment

"This is the same as my experience. PIDs Parameters are one thing, and Rate Parameters are another. Take a car as an analogy.  "PID" would be like tuning the engine, so that fuel, air, timing are correct (things in the Flight Controller).  "Rates" are like tuning the steering wheel, pedals, gear (stuff you directly touch like the Transmitter Sticks) so that the 'feel' is correct. And adjustment of the Rates itself made a huge difference on how 'sensitive' the aircraft feels, especially to a noob like me"- Kuson comment

d. Battery Factor: "A while ago someone took over my pids to his quad with same setup and he said it didn't feel good. So I flew his setup and it indeed felt like PIDs were twice as low as they should be! It appeared he was using almost 2 years old (Turnigy) Nanotechs completely lost their power. Even I feel huge difference between different batteries I have." - Boris comment

##Why does my copter behave erratic after a crash ?
Some people have experienced erratic behavior (jitters etc) after a crash,  as if P went up significantly.

"When you crash your gyro can get upset. It has always been like that even from Baseflight days.
Some gyros are more sensitive than others.
To Recalibrate Gyros: " Disarm. Perform gyro calibration (left stick down left....right stick down) and it will be fine. You will see leds blinking and it will beep. Also when plugging your lipo in your quad, * your quad should not be moved*." - Boris comment

If you fly in Auto-level modes (Angle or Horizon) then the Accelerometer can easily get upset and gives False readings. The Accelerometer can also get upset if there are excessive vibrations or during fast Aerobatic moves. 
Not much you can do except wait for the accelerometer readings to settle down. This is easily seen if you raun a minimOSD with the Artificial Horizon displayed.

##How does yaw_jump_prevention_limit work ?
"First you need to know the basics of a mixer function on multirotors.
Mixer gets PIDsum of all 3 axis and translates that into motor output.
There is obviously a certain power available there, which is a range of max_throttle - min_throttle for each motor.
Lets say it is typically a range of 1000 (2000-1000).

So we have 4 motors determining the behavior of quadcopter with power range of 1000 for all 3 axis mixed up scaled to the throttle.
The yaw axis is the one what requires quite a lot of power on quadcopters but also depends of setup used. The ratio of power used for yaw to get the same rotational rate is more than one for pitch and roll.
This means that hard yaw corrections could use too much of the entire available throttle range in each motor so the roll and pitch would not have enough of it. But also the way of how yaw works can create a lot of thrust where the quad would gain height to get a desired yaw correction during yaw stops what generate the most power.

Therefore the yaw_jump_prevention_limit was introduced to give a maximum of yaw PIDsum with centered sticks. That means that during yaw correction, the yaw is not able to use too much of available motor power so the roll and pitch would not be affected as much and that also the hard yaw stops would not create a lot of jump.

Lowering yaw_jump_prevention_limit will result in less motor power spilled for yaw during gate clipping for example as well.

You still have the full yaw control when using stick input.

But anyway I am still surprised that your gear suffers from jump. I would say that small...and powerfull x quads would typically not suffer from jumps." - Boris B

##How should I configure the FailSafe system ?
FailSafe is something that needs to be configured in the radio receiver and the Flight Controller.
Take a look at this overview as it describes how this should be done: http://www.youtube.com/watch?v=dikr9oDzQqc

Some additional information can be found from 6:20 onwards in this video: http://www.youtube.com/watch?v=htkw7d97bOo

NOTE: Failsafe configuration has changed in Betaflight 2.4.0 onwards and CF Configurator 1.2.0. The relevant documentation can be found [here](https://github.com/cleanflight/cleanflight/blob/master/docs/Failsafe.md). 

##What is the best practice for configuring the Throttle end points ?

For KISS ESCs:  
Just cal with max_throttle at 2000 and min_command at 1000 from the CF config Motor tabs.

Joshua's method for BLHeli 14.4 and lower.  
This can be a difficult and confusing concept to grasp at first. The best way to describe the correct method is by way of the following tutorial video.

Part 1: http://www.youtube.com/watch?v=WFU3VewGbbA

Part 2: http://www.youtube.com/watch?v=YNRl0OTKRGA

New information concerning BLHeli Max_throttle calibration.

Watch these Videos from Joshua Bardwell  
Cleanflight BLHeli Top-End Throttle Calibration : https://www.youtube.com/watch?v=spegUYF8Dxk
The Effect of Top-End CleanFlight BLHeli Throttle Calibration : https://www.youtube.com/watch?v=RW2XalNPpQk

The new BLHeli Firmware v14.5 has some differences in calibration from 14.4. Joshua B recommends:  
Calibration procedure I currently recommend for BLHeli 14.5 is min_command=1000, max_throttle=1980, run calibration. Done. Top-end calibration may not be needed since BLHeli 14.5 seems to have much less or almost no dead band at the top.

Using max_throttle=1980 is because everybody seems to be getting 2020 as the calibrated value if they use max_throttle=2000. So the goal of using 1980 is to ensure that you calibrate below the top of the scale.

Another approach I have seen suggested is to set min_command=1000, max_throttle=2000, and then manually use the Motors tab to determine the spin-up point and max rpm point for each motor, then manually set those values into the ESCs. This is a fine approach too, but a little more intensive.  
Then compudaze posted:  
I've had some ESCs still hit 2020 using 1980. Been using 1970 as it doesn't hit the max. Tested with LB20, RG20, UBAD30, XM20.   

New Video from Joshua Bardwell titled "BLHeli - 100% Explained".   
https://www.youtube.com/watch?v=0Bi1XcdpnQI  

##How do I configure BLHeli ESCs via BetaFlight ?
If running at 1kHz and faster BLHeli 14.2 or later is required and disable PWM in the BLHeli configuration. This is to ensure the BLHeli Firmware recognizes the OneShot125 pulses properly.

If you are running BetaFlight, you can program and flash your BLHeli ESCs (that have BLHeli bootloader only!) directly through the flight controller, without disconnecting the signal wires or disassembling the copter at all.

As a rough guide to determine if your ESCs can be flashed via BetaFlight:

| ESC MCU manufacturer | ESC Firmware | Bootloader Type | Flash via BetaFlight |
| ----------------- | ------------ | --------- | ------------------------------- |
| ATMEL | SimonK | SimonK | N |
| ATMEL | BLHeli | SimonK | N |
| ATMEL | BLHeli | BLHeli | Y |
| Silabs | SimonK | SimonK | N |
| Silabs | BLHeli | SimonK | N |
| Silabs | BLHeli | BLHeli | Y |

In general, it's all down to the ESC having BLHeli Bootloader so that ESC flashing can be done via Betaflight.

Follow this guide to learn more: http://www.youtube.com/watch?v=YWLk4qcQcvw

PLEASE NOTE: This does not work on the following boards:

| Flight Controller | Failure Reason                                      |
| ----------------- | ----------------------------------------------------|
| Moto Tornado | Since the 5v buffers on the motor outputs are uni-directional and do not support bi-directional communication. These buffers make the motor outputs more stable, but prevent passthrough. There is no software fix for this. The only fix would be a re-design of the board to remove the buffers or change them to bi-directional buffers. |
| Naze32 **Rev6** | The Naze back-fed the ESCs from the USB port. So the ESCs would power up, see the throttle signal, initialize, and then they wouldn't go into programming mode after that. The Rev6a has fixed this issue since it was released in November 2015 |

##Why does my copter flip when trying to takeoff ?
Here are some likely causes:

* Motors plugged in to the wrong FC headers.
* Custom mix is incorrect.
* Motor spinning the wrong direction.
* Props on the wrong motor.
* Flight control board mounted facing the wrong direction (e.g. yaw 90 degrees left but the board_align has not been configured to reflect this).

##Will the PIDs change significantly when switching from two-blades to tri-blades ?
Some have found they need a small reduction in P gains when going from two-blade to tris.
The copter was still flyable with no changes, but some have experiences increased prop-wash oscillation.
 
##Why do I have issues flashing my new F3 Flight Controller ?
Some of the new F3 boards come with a Virtual COM Port (VCP) that is used to communicate with a PC or MAC over the USB interface.  Take a look at this video that talks about flashing the Lumenier LUX board that has a VCP port:
http://www.youtube.com/watch?v=b8fMsazyxDw

Within this FAQ, check the answer to "What Flight Controllers are recommended to get the best out of BetaFlight" for more details on which FC has VCP ports.

##Will Betaflight code be merged back into Cleanflight ?
Yes, it is the intention that this will happen gradually over time. Sometimes features from CleanFlight also get merged into BetaFlight too. This code merge (in both directions) has already started happening from BetaFlight V2.4.0 and CleanFlight V1.12 onwards.

##When I update to the latest version of BetaFlight do I need to recalibrate my ESCs ?
ESCs shouldn't need recalibration unless you changed the min/max throttle values in BetaFlight.
For more information about ESC Calibration see this video: http://www.youtube.com/watch?v=o3Mg-9M0l24

##Why do my motors keep accelerating on the bench when I arm without props ?
With props off on the bench, I arm the quad and the motors start. After increasing throttle a small amount then back to minimum I notice the motors keep increasing in speed.  They don't go to max or anything, but they climb noticeably. Now if I was in Angle/Horizon with the accelerometer enabled I could understand that the quad was tying to level itself. But in Acro mode why should the throttle change on its own ? I'm guessing this is an Airmode effect. But just wanted to understand a little more about why.

Answer: That is the flight controller trying to correct for changes in aspect, mainly due to fact your quad shakes slightly when the motors spin, the sensors pick it up and then the flight controller tries to correct, it can't because you don't have props on. All perfectly normal.

Additional explanation:

 Originally Posted by MasterZap View Post
Sorry, but this sounds like a fundamental misunderstanding of how the I term works.

Or conversely, the behavior you see on the bench is exactly expected of the I term.

Why? Because the copter isn't moving. If there is no movement, you have no gyro input. With no gyro input, there will be no positive (or negative) error signal to add to the I term.

The I term is additive. As error is measured, that error is added to I. If error persists, I grows. If error STOPS, I STAYS. Only at NEGATIVE error does I shrink back down again.

Since your copter isn't flying, you are only giving it half of the error (your stick input tells the copter to rotate x degrees a second, the copter is rotating no degrees per second at all, hence you have an x degrees per second "error" measurement) and I will grow. In the air, the copter would start rotating, error would shrink, and eventually become negative and decrease the I term back.

So perfectly normal.

You simply cannot make judgments on an I terms behavior without letting that I term act the way it wants. With props off, on the bench, you just get meaninglessness.

/Z

A quick way to test that there isn't some other issue causing it is use the motor test page to remove the PIDs from the equation.

##Why do my motors spin briefly when rebooting the Flight Controller ?
Since flashing 2.4.0 and rebooting from Configurator with a battery plugged in spins up the motors briefly. I'm fairly sure that didn't happen in 2.1.6, not sure about 2.3.5.

Answer: This can happen in any firmware with battery plugged in. It can happen in 1 out of 100 times or every time. Thats not a bug....that's how OneShot works.  The ESC would interpret a small pulse during power up and down as a signal and spin motors.  It is really a short pulse what couldn't really harm anything but still can scare the s**t out of you !

It is also highly recommended to always use a Current Limiter when the LiPo is connected and the Config Gui is opened. This can prevent burning ESCs and motors. See: http://www.rcgroups.com/forums/showthread.php?t=2327875

##If the accelerometer is disabled and FailSafe Activates what happens to the copter ?
It cannot do self-leveling without the accelerometer sensor activated, so it won't Self-Level it will just tumble to the ground.

It is recommended to setup Fail Safe to disarm (shut off motors) immediately upon entering Stage 2 and allow copter to Drop if the Accelerometer is disabled.

##Why does my Flight Controller blink/beep lots of times when powering up ?
5 short blink/beeps followed by any number of long blinks/beeps indicates an error code.
Number of long blinks indicates the following error:

1. ***FAILURE_DEVELOPER***: External interrupt of sensor failed to initialize.
2. ***FAILURE_MISSING_ACC***: Accelerometer/gyro sensor is missing
3. ***FAILURE_ACC_INIT***: Accelerometer/gyro sensor failed to initialize
4. ***FAILURE_ACC_INCOMPATIBLE***: The found accelerometer/gyro sensor is not compatible/not the expected one
5. ***FAILURE_INVALID_EEPROM_CONTENTS***: EEPROM/FLASH configuration content is invalid
6. ***FAILURE_FLASH_WRITE_FAILED***: Write of configuration to EEPROM/FLASH failed
7. ***FAILURE_GYRO_INIT_FAILED***: Gyro initialization of SPI MPU6000 accelerometer/gyro failed

The most common one seem to be error 2 where the accelerometer/gyro sensor can't be found, this is caused by a bad sensor or bad connections to the sensor, could happen because of a bad crash. On most boards gyro and accelerometer is the same chip so acro flying isn't possible when the accelerometer isn't found, it's not just the accelerometer that's bad but the whole chip.

Error 3, 4 and 7 could also be caused by a bad accelerometer/gyro sensor. 
Error 5 and 6 indicates memory read/write problem of the MCU (main processor).
In most cases a new flight controller board will be needed if the user isn't for example able to re-solder the sensor.  

Above are Hard Faults the Processor detects upon boot-up and initialization. Additional reasons for flashing LED and/or beeping are:  
  No signal from RX. This could be simply the TX is off or the wrong Model/binding selected or a hard fault of the RX like no power or bad cable.  
  Accelerometer Not calibrated if the ACC is enabled (check the CLI). If acc is enabled then it must be cal'ed once and typically done in the config GUI.  
  Copter titled too far if the Acc is enabled. 


##My PID D gain value is small after tuning in 2khz mode is that normal ?
The latest 2KHz versions of Betaflight seem to be enhancing the influence of P, to the point where you can fly with good P gains and very little D. It's also good practice to keep the D gains low so that the motors don't get too hot with all the rapid speed changes.

##Why are the accelerometer Black Box traces so bad in 2KHz mode ?
With my quad on the ground, 1Khz, no props, motor-stop, the accelerometer traces are smooth x=0 y=0 z=1. There is just the tiny amount of noise you would expect from the chip itself.
On 2Khz, the data in BlackBox is nonsense, eratic X=7G, 3G, 5G all over the place. The quad is stationary on the ground, the motors aren't spinning, is this aliasing ?

Answer: Yes, this is really effects of aliasing what you are seeing there. Acc has nothing to do with 2khz....it is same with any gyro rate. We are just undersampling it on 2khz.
If you use Level/Horizon modes then just stick with 1khz or get some very fast F3 target....one that will do full sampled acc even on faster rates.

##How do I get vbat_pid_compensation system working ?
    set vbat_pid_compensation = ON

Tune your quad with a full lipo....your PIDs will then be scaled to that reference voltage.
Voltage scaling from full lipo to empty is limited to 25%. Should be enough as we fly to 3.3v usually.

Also good when you have old and new lipos. The old ones with more voltage droop will automatically get more PID adjustments.
It also disables itself when voltage completely drops below 2 cells

Forum Question from eL_Verde: 

I tried your PID voltage compensation. It felt good, but I think for me, and my setup, that 25% plus over gain when the voltage is low is a little high.. Can I mod this value for 20% or 15%?? 
Boris' Answer:

voltage gain is adjustable
- max voltage = 1
- min voltage (default 3,3) = min adjustment. It is even more than 25%!
- When you raise min voltage up you will get less compensation
I also think that vbat compensations helps against prop wash as the motor gives more constant power during power dips. Those are the main reason of prop wash weird oscillations besides air effect 

**Note:** This requires VBAT connection on the FC (LiPo pack voltage) and VBAT Feature Enabled.

##With vbat_pid_compensation are there issues moving from 3S to 4S batteries ?
There won't be a problem, the cell count is calculated and the PID adjustments are based on the Cell voltage.

##How can I run the PID controller faster than 2kHz ?
### Instructions for ßF V2.5.0 RC6 and later
Set looptime (microSeconds) in config GUI.
OneShot42 and MultiShot now supported

2 examples of auto config

looptime 125

- always 8khz gyro sampling (gyro_sync_denom = 1)
- when just oneshot125:
- pid_process_denom =3
- when use_oneshot42 or use_multishot
- pid_process_denom = 2

looptime 250

- always 4k gyro sampling (gyro_sync_denom = 2)
- pid_process_denom = 2
- on f1 boards with luxfloat
- pid_process_denom = 3

etc....

motor update speed = pid speed
calculation of motor speed:  motor update interval us= 125 * gyro_sync_denom * pid_process_denom 

PID is always synced to motors! PID speed is immediately your motor update speed.  Gyro can run faster than PID. The benefit of that is the higher sampling reduces filtering delays and helps catching up all higher frequencies that may fold down into lower frequencies when undersampled.  Even when GYRO runs faster than PID it is still in sync, but every (pid_process_denom)th sample.

###Instructions for ßF versions up to 2.4.1
TODO
- FC Settings?
to change the refresh rate, one way is go into the CLI and change the Gyro denominator setting.
For 2khz (500usec), set it = 4
for 2.6khz (375usec), set it = 3
for 4 khz (250usec), set it = 2 (iffy depending on ESCs)
for 8 khz (125usec)  set it = 1 (not recommended)
Or just set the Looptime in the Config GUI. Note that only 5 looptimes are supported, 1000, 500, 375, 250 & 125usec.

- Which FCs does this work on and how fast can they run the PID controller/Gyro readings?

- Which ESCs and on which Motors?

Not all ESCs can accept a faster refresh rate. This can also depend on the motor kv rating. Since not many have run looptimes this fast it is best to read threads in RCG on the ESCs you are using to see what refresh rates and motor kv may work.

- Running Looptime at 250usec (4kHz loop rate) and OneShot125. How to prevent 'no pulses' at max throttle.

Since OneShot125 has a maximum pulse width or 250usec this will not work if the looptime is also 250usec. The FC will never set a logic low to have a gap between pulses if max_throttle = 2000usec (OneShot pulse width = throttle output/8). One way to get around this is set the max_throttle to a lower value and Cal the ESCs to this value. Max_throttle = 1850usec should work (one person used this and it works). This allows 150/8 = 18.75usec gap between pulse at max_throttle. This is being called the "Short Cal" of ESCs in the forum threads.

So far OneShot42 is not supported in ßF YET but would allow 4kHz refresh rates. Check MultiShot, RaceFlight & BLHeli_S firmware.

**Important Note: With fast Loop rates there have been reports of the Gyro Caling during Bootup much sooner and the copter could be moving due to connecting the battery. If the copter is moving during Gyro Cal then the bad things can happen. Do observe the LEDs for very fast blinking or the three beeps. If you think the copter was moving during the Gyro Cal then just do a manual gyro cal with the Stick command, Hold minimum throttle and YAW then cent Roll and minimum Pitch.**

##What is OneShot125 OneShot42 and MultiShot and how do these relate to max_throttle and Looptime ?
TODO
With the Standard ESC calibration to min_command = 1000 and max_throttle = 2000.
OneShot125 will send pulses to the ESCs that are 1/8th the Standard values of 1000 to 2000 or 125 to 250usec.

 Originally Posted by HIGHOCTANE32
Once you wrap your head around and think about looptimes and ESC pulses(whether the be 1000-2000us pwm, or 125-250uS oneshot or whatever as time(which they are) it all makes a lot more sense. Trying to sync a gyro rate that updates every 125us (8khz) or even 250uS with a ESC signal pulse that can be 250uS long..you can see the problem. Oneshot 42 and multi shot further shorten the ESC signal pulses, like oneshot 125 did, but even shorter, so the signal pulse can be completed faster than the gyro/PID update. Not a scientific explanation but hopefully that makes sense.
But I agree if josh doesn't already have a video on it he needs one 

Some info here on Oscar Liang's excellent Blog site regarding MultiShot technique:
http://blog.oscarliang.net/raceflight-multishot/

##What cycle time can I run on what board ?
F3 i2c targets:
 250 cycletime without acc, you can enable acc mode, but watch out for CPU usage when many features enabled. Anyway I recommend going to 2.6k when using accelerometer.
 Also boards with baro or mag on it even when disabled may decrease performance a bit.

 F3 spi targets:
 Not much worrying here. 125us looptimes and accelerometer and everything can be on. But always check CPU to be sure.

 F1
 With rewrite or mw23 250us cycle should be possible without problems on NAZE32 and all clones of it like flip32 etc when accelerometer disabled.
 Especially the boards without fancy sensors like baro or mag should run super smooth. The boards with baro even when disabled may have a bit higher cpu times.
 For level modes I recommend 1k mode. Those users don't necessary have to run 188hz gyro lpf....they still can set gyro_sync_denom to 8 to minimize latency by 1 millisecond.

 CC3D users should stay with 375 or 500 cycletimes.

 And all F1 users on luxfloat should not go lower than 500 or 375....probably even 375 is too much.
 With acc enabled stick to 1khz. 
(From BorisB)

##How do I go about suggesting CF Configurator enhancements ?
1) On GitHub, look up the Cleanflight Git. There's a link to the Configurator.
2) Click on "Issues".
3) Start a new issue and preface it with "Suggestion: short summary".
4) Explain the new enhancement suggestion.

##How do I lower the chance of my copter producing Magic Smoke when powering on ?
Start by doing a continuity check with a multimeter if you have one.  A quick test for a short between the negative and positive pads on your power distribution board can save a lot of headaches.

Another option is to use a Current Limiter when having the LiPo connected on the bench and Testing new setups. This has saved a few ESCs and Motors for many people.  Build and use this Limiter with a Switch in-line for easy powering On/OFF.
http://www.rcgroups.com/forums/showthread.php?t=2327875

##Why do we have RC Rate and also Yaw Pitch Roll Rates ?
Deeper Question: There is still some confusion about RC rate, Pitch, Roll, Yaw rate, and Expo. I understand that P/R/Y rates are how fast the quadcopter will rotate, and i know about expos too, but what is really RC rate? I can't really gain a full understanding of it. Some say it does the same as P/R/Y, some say it's different from it, some say it's stick sensitivity. But what is stick sensitivity really? Is it like expo?

Answer: Think of it as fine tuning for RC Rate. It does the same thing just smaller increments and splits the axis up.

Some people leave the RC Rate set to 1.0 and adjust the P/R/Y rates until the quad handles how they like (speed of flips/rolls etc). Once this has been set, the Expo values should be increased to allow for less sensitivity of the sticks nearer their center positions. This will make for smoother flight experience, and have the ability to perform fast rolls etc when the sticks move further away from the center.  This is the best way to do it at the moment.

##Why does it matter to prevent motor jitter ?
Two reasons:
* The motor is stop starting, this will generate heat and potentially damage/wear out components.
* As above your motor is stop starting, it isn't providing the thrust it is supposed to, your quad will shake/oscillate/crash and generally be unflyable.  See the Deep Dive page for a more in-depth explanation.

##Why when I change something using CLI board crashes ?
If the FC uses the STM32's VCP then when leaving the CLI the config GUI does a "save" which re-boots the FC. Then Windows does not reestablish the USB. Check in the Device Manager to see if the Port has returned. If not then a work around is to disconnect and reconnect the USB. On some PCs/FCs this doesn't work so plug the USB into a different USB port on the PC. I keep two USB cables plugged into a Powered USB hub and just swap the USB cable to the FC and the Port comes back in the Device Manger and the Config GUI now sees to port.
This is NOT and FC or Firmware issue but a Windows USB issues.

##Will MW23 PID controller work on default PIDS ?
No! Even though Boris believes this is now the best flying PID controller, it will not fly correctly on default PIDs much like rewrite and Lux will. You need to manually tune this like the good old days.
In BorisB's words from Regroups

"Guys I read a lot of comments about bad defaults for MW23 pid controller. I will say it once more......there are NO defaults for MW23. The defaults are made for rewrite actually. It is not possible to have defaults for both....rewrite and MW23.
You really have to tune that one by yourself."

Reports show that default PIDS are too high. Be careful when first arming as it might have serious oscillations.

The key takeaway is:
* P gains need to be less than on MWREWRITE
* YAW Rate needs to be lower than on MWREWRITE
* Roll & Pitch Rates needs to be higher than on MWREWRITE

**Originally Posted by Boris B**

D is quite tolerant it appears. I scaled it to the looptime which wasn't there in the first place.

The first time I started testing the lower looptime was getting the lower I and D was needed. Now it is normalized to looptime ~2000 to give values close to original multiwii..

Iterm is more aggressive though. It can even cause oscillations, which finally makes Iterm tuning easier.

These are my PIDs. Not fully tuned though as I focused more on firmware testing:  
Roll 3.0   0.025   22  
Pitch 3.5  0.035  35  
Yaw 5.8    0.045   0  
RC Rate 1.0
Rates 0.7 0.8 0.8
RC Expo 0.2
Rc Yaw expo 0.3

Level (I don't really fly level but had to test it as level also has I and D):  
Level P 9.0   I 0.005   D 0

Additionally:
Rates about .7 as a start
yaw rate 0.8
RC Rate 1
rc yaw expo 0.3
rc expo 0.3

Angle and Horizon modes still need some work

Don't forget to follow this good approach to tuning your multi-rotor:
http://github.com/borisbstyle/betaflight/wiki/PID-Tuning-Guide

##How do I keep and then restore my Betaflight Settings each time I upgrade ?
First of all it is important to note that uploading a **full** settings Dump from a previous Betaflight version will likely result in your copter not flying properly, not flying at all or even damage to the components.

It's also worth noting that the method of flashing Betaflight **can** be dependent of the FC board. So best to refer to the thread on the FC board you are using. The list of Boards in the FAQ have links to these threads.
Any issues/differences in updating are typically listed in the Release Notes which are a MUST READ.

Having said all this, one approach worth considering for ensuring your settings are migrated from one Betaflight version to another is described in these videos:

http://www.youtube.com/watch?v=HsxTqp76Brs
http://www.youtube.com/watch?v=F1sjC5l0ywM

In summary, the key takeaways from this video are:
* Keep a separate custom config file that just has the settings that you have invested time in getting correct for the flying experience you want (PIDs, rates, AUX switch settings etc).
* Upgrade the FC to the desired Betaflight version then uploaded your custom config file.
* Ensure the custom config file is up-to-date with PID & Rate values during and after tuning. This way you can compare tuning and/or restore a tune if you changed firmware versions and need to go back.

Here are a few tools that are useful for making comparisons between config files:

1. Notepad++ with the Compare PlugIn
1. https://www.diffchecker.com/

##What is yaw_jump_prevention_limit and what does it do ?

From Joshua Bardwell

Yaw jump prevention limit puts an upper cap on the yaw P term when the yaw stick is centered. There is a problem when you do a big yaw move and then suddenly snap to a stop where the copter has low yaw authority, so the copter cannot respond as quick as it wants to, so error grows large and the P term grows large and the motors surge like crazy at the end of the sharp yaw move. Lowering yaw_jump_prevention_limit will soften the end of sharp yaw moves, but will prevent the motors from surging and the copter from jumping. Raising yaw_jump_prevention_limit will sharpen the end of yaw moves, but will result in the motors surging if you don't have enough yaw authority. If you have a high-performance copter with great yaw authority, and if you want snappier endings to your yaw moves, raise this value as high as 500 (disabled). Remember that this only affects the end of yaw moves, because it only applies when the stick is centered.

Addition from Adam Pyschny

A to low yaw_jump_prevention_limit can prevent yaw P from getting enough authority to prevent the quad from breaking out in tight, high speed, roll-only turns.  

##What is yaw_iterm_reset_degrees and what does it do?

From Joshua Bardwell

yaw_iterm_reset_degrees determines the number of degrees above which the Iterm will reset to zero and stay there. the units are degrees per second rotation and they go from 25 to 1000. The issue here is that, on extreme acro moves like flips and rolls, the I term can accrue error, and then at the end of the move, the I term trying to unwind that error can result in rebound or overshoot, instead of sharply stopping the move. This parameter causes the I term to zero out when the rotational rate goes over a certain value. The idea is that, in a flip or roll, you don't care about correcting for persistent bias on that axis. You just want to flip or roll close to the targetted angular rate.

##How does Super Expo work ?

From BorisB

Super expo is similar to acro plus, but acro plus was adding more rotation rate outside the pid controller and the pid controller would fight against that. That didnt really feel natural somehow.  
Super expo manipulates the pid controller so it does expo for you.
It actually works as an acceleration to P based on higher stick input and deacceleration with lower stick input. That also provides more clean acro and less need for D in general.  
If you have your quad tuned for mid stick you have now....that pretty much stays the same and maybe even a bit softer, but it accelerates towards the full stick.  
Its like multiwii implementation but with your current rates so you still can have snappy mid stick that what multiwii is lacking a bit.

Besides that, betaflight 2.6 allows much higher D without noise so you can get it smooth easier anyway.

Another explanation from Joshua Bardwell

super_expo_factor works like this. Normally, the way the PID controller works is that the stick position commands a certain angular rate, and then the difference between the actual angular rate and the target angular rate is used to calculate an error value. The P term is proportional to the error value. The larger the P term, the stronger the motors' output to achieve the commanded change in angular rate. Got all that?  
But with super expo, the way it works is that, the more deflected your stick is, the more the P term is directly proportional to the stick position, instead of the error value. So as you deflect the stick, the PID controller says, "I don't care what the current angular rate is, or what the error is, just push so hard."   
Here is an analogy. Normally when you drive a car, you are looking at your target speed. Say it is 55 mph. And if you are going faster than that, you back off the gas pedal, and if you are going slower than that, you push on the gas pedal. That's the way the PID controller works. But super expo is like saying, "I don't care how fast I'm going. Push the throttle to 75% and just keep it there."   
As with the I term reset, the idea here is that, when you're commanding extreme maneuvers, you don't care about hitting an EXACT angular rate, like 1234 degrees per second. As long as the copter's behavior is reasonably predictable, you would rather let it "loosen up" a bit and just spin. If you look at Blackbox during a flip or roll, the P term is often switching signs several times. So it is trying to slow down the roll and then speed it up and then slow it down, and that's a bit silly to all be happening in the course of 0.2 seconds while you're flipping around.

A Boris comment:
Just one additional thing about iterm reset.
In super expo mode the iterm is also being reset on roll and pitch above certain deg/sec (default 200)
That is really necessary as super expo gives some P acceleration and Iterm would start to windup even more as it would think that Pterm is doing a bad job.  
The iterm again becomes active below the threshold rate and gets to normal levels in time without you notice anything.  
Removal of iterm during faster acro manouvres provides more connected feel as all stickyness from Iterm is removed. 

Video explanation:
https://www.youtube.com/watch?v=HGAa8J1Ihac

##How do rates relate to pitch roll & yaw degrees/s ?

MadmanK has written a spreadsheet to show you pitch roll and yaw rate in Rewrite and Luxfloat to show how it relates to your rates in degrees per second.
Only change the values in the grey boxes, and it will adjust the graphs and tables.

[Rewrite/Lux rates](https://dl.dropboxusercontent.com/u/31537757/Betaflight%20Rates%20v1_4.xlsx)

##Which Flight Controllers currently use SPI ?
  
  
As of 5th May 2016  
Colibri Race  
Lux Race  
Motolab Cyclone  
SPRACINGF3EVO  
DOGE  
CC3D (this is F1 board though......performs slightly better than i2c F3 board on rewrite)Alienflight 

##Which HEX target do I download and flash to my Flight Controller ?

Sometimes it's pretty obvious which Betaflight HEX file to download and flash to your Flight Controller (like NAZE) but other times it's not (like RMDO). It's also worth noting that some of the HEX files are used with multiple FCs (like clones for instance).

We typically just say HEX file but many targets also have a BIN file. These are just for different Flashing software tools. Use the HEX file in the CF config GUI and when un-bricking with the ATM Flash Loaded tool. The BIN files are used with other tools like the Linix DFU flash utility. See the threads/instructions for individual FC boards to find out if and how a BIN file is required.

Having all the Flight Controllers listed here (and their associated HEX/BIN) should help avoid some confusion.

| Flight Controller | HEX/BIN File |
|---|---|
|[AfroMini Naze 32](http://www.readymaderc.com/store/index.php?main_page=product_info&products_id=4406)|AFROMINI|
|[Alien Flight F1](http://alienflight.com/)|ALIENFLIGHTF1|
|[Alien Flight F3](http://alienflight.com/)|ALIENFLIGHTF3|
|[Flip32/Flip32 Pro](http://www.readytoflyquads.com/flight-controllers/flip-series)|NAZE|
|[DTFc Flight Controller](http://rotorgeeks.com/index.php?route=product%2Fproduct&path=34_44&product_id=555)|DOGE|
|[DragonFly32/mini/compact](http://www.multirotormania.com/129-dragonfly32)|NAZE|
|[FuryF3](http://www.2dogrc.com/furyf3-board.html)|FURYF3|
|[ImmersionRC Fusion](http://www.immersionrc.com/fpv-products/vortex-250-pro/)|IRCFUSIONF3|
|[Lumenier LUX](http://www.getfpv.com/lumenier-lux-flight-controller.html)|LUX_RACE|
|[Motolab Tornado](http://impulserc.com/motolab-tornadofc-stm32f3-flight-controller)|MOTOLAB|
|[Motolab Cyclone](http://impulserc.com/motolab-cyclone-stm32f3-flight-controller)|MOTOLAB|
|[Naze32 Acro](https://www.nextfpv.com.au/products/naze-acro-flight-controller-white-genuine)|NAZE|
|[Naze32 Full](https://www.nextfpv.com.au/products/naze-32-flight-controllerred)|NAZE|
|[OpenPilot CC3D](http://www.getfpv.com/openpilot-cc3d-flight-controller-straight-pins.html)|CC3D_OPBL|
|[RMRC Seriously Dodo](http://www.readymaderc.com/store/index.php?main_page=product_info&products_id=4221)|RMDO|
|[Singularity](http://singularity.impulserc.com/)|SINGULARITY|
|[Sparky](http://www.readytoflyquads.com/sparky-flight-controller)|SPARKY|
|[Seriously Pro Racing F3](http://seriouslypro.com/spracingf3)|SPRACINGF3|
|[Seriously Pro Racing F3 EVO](http://seriouslypro.com/spracingf3evo)|SPRACINGF3EVO|
|[Seriously Pro Racing F3 Mini](http://seriouslypro.com/spracingf3mini)|SPRACINGF3MINI|
|[TBS Colibri Race](https://www.multirotorsuperstore.com/controllers/controllers-by-firmware/baseflight-cleanflight/tbs-colibri-race-flight-controller.html)|COLIBRI_RACE|
|[X-Racer F303 V1 -> 2.1](http://www.fpvmodel.com/x-racer-f303-flight-controller_g1106.html)|SPRACINGF3|

##How do I setup for reversed prop rotation ?
Just change props and motor rotation in BlHeli.   
Then change set yaw_motor_direction = -1   
Remember to cycle power to FC so new setting become properly used.

##What is a recommended FC and esc setup to run at 8khz also i see reference to 4/4 or 4/4/32 or 8/8, what are these referring to ?  
First number is gyro freq (set by looptime, 1000=1K, 500=2K, 250=4K, 125=8K),

Second number is PID calc freq, this is set with regards to looptime, pid denom 1=same freq as gyro,
pid denom 2=half speed of gyro,
and so on.

Third number, its esc update rate, if no number, its the same as pid calc freq (in sync).

In BF v2.7.0, you need to:
set unsynced_fast_pwm=ON
set fast_pwm_protocol = MULTISHOT
Set motor_pwm_rate = 32000

Oneshot125 up to 4K (125-250μs)  
Oneshot42 up to 12K (42-84μs)  
Multishot up to 32K (5-25μs)  

Generally, depending on pidc, serial ports used, number of Rx aux channels, etc. The acc is disabled in most scenarios below.  
F1's mostly run between 2.6K - 2K, if you get a $9 cc3d they run 4K/4K, ccd3d-F3 run 8K/8K.  
F3's mostly run 4K/2K but can run lux pidc and has more serial ports.  
F3's with spi gyro (LUX, etc) can run 8K/8K.  
F4's (revo/etc) on raceflight can run 8K/8K, if using the 6500 or 9250 gyro(sparky2/etc), they are just now starting to run 32K/32K/32K.  

All these FC can run esc up to 32K esc update rate at no extra penalty. Always check cpu usage via cli command "status", I prefer to stay under 30% cpu on BF, some get away with more.  

##What is the difference in PIDC Iterm in ßF versions ? 
By ctzsnooze:  
Any slow pitch back type thing is iTerm related. Pitching back means that P alone was unable to retain the intended angle in FFF, and iTerm accumulated in an attempt to get there. When dropping throttle, the need for that amount of iTerm changes, and it takes a short time for the iTerm to drop back. In that sense this is a symptom of P not being quite enough, or I being too much.  

However in some situations ITerm accumulation is inevitable and the challenge is how best to deal with it.  

In 2.6 code was introduced that set iTerm to zero once gyros indicated a certain level of rotation. ITerm didn't start accumulating again until gyro rate fell back below the threshold. This controlled excessive iTerm gain but caused a small but unwanted step change in pitch at the time of returning back past the threshold.  

In 2.7 this was changed to not reset to zero but to hold the value iTerm was at when the threshold was crossed. That also caused similar issues on return to normal as it abruptly changed whatever the newly required iTerm value would be.  
Boris: No in 2.6 and 2.7 there was still iterm reset like in 2.6 actually, but only in super expo mode or when forced in cli. It wasn't in the normal scenario!   

2.8 has code that reduces iTerm accumulation the higher the roll rate, but never arbitrarily cuts it to zero. If the threshold is lowered, high roll rate events have less impact on iTerm, but iTerm keeps working normally at low roll rate periods eg in FFF.  

Could I suggest that people with this issue first try a bit more P, if that's possible, but if more P isn't ideal, maybe try reducing the iTerm ignore threshold to say 50 or maybe even 25. This has the effect of reducing iTerm during high roll events and may improve the situation without reducing iTerm's ability to otherwise keep the quad stable. If dropping the threshold means an overall inadequate I level, try increasing I at the same time.  

These parameters can be varied quite a lot in attempting to find the best value. But the best solution is to have a quad where P is enough to get the angle you want mostly by itself.   

##How to setup blackbox record rate with onboard dataflash ?
Be carefull when setting up blackbox record rate with onboard dataflash.
When running at the edge of the board (like 4khz/4khz/4khz on sp3 board), there is a risk of overunning the cpu with too high rate like 1/1, even 1/2.
You need to test on the ground without props and check cpu usage, so just arm, activate blackbox, and check status on the cli command.
Keep a safe value and leave some room for cpu usage.
1/4 should be a correct value for sp3 board at 4khz/4khz/4khz.

##How to setup the rates and SuperExpo in ßF 2.8.1
- First see the Rate calculator in the 2.8.1 Release notes and Watch Joshia's video on ßF 2.8

Originally Posted by Boris B View Post  
I have explained it many times in this thread.  
RC Rate / RC yaw rate = Mid stick feel  
Rates = Far stick feel  
Rc expo / yaw expo is actually not necessary at all.  
Just adjust till it feels good.  

#### Boris: 
Didn't I explain already 3 times that he just needs to enter one command to disable the feature and have old fashioned linear rate between 0 to 2000deg/sec
The choice is there for everybody and different styles. There are no assumptions.

Note: Super Expo uses floating point math and when enabled uses much more CPU cycles. This mean on F1 and F3 with IIC gyros targets the looptime might need to be reduced.
Boris states: There is more than just super expo.
Just lower the looptime on F1 boards. 4k doesn't make it fly better.

#### compudaze's method:  
what I did for 2.8 was adjust rates until my deg/sec at 2000us matched what it was for 2.7 with super expo. Then I adjusted rc rate until my deg/sec at 1750us matched what it was for 2.7 with super expo. It's not that simple though as you'll need to keep playing with the number to get it exact. just keep in mind rates more effects end stick while rc rare more effects center stick. 

#### More from Boris:   
Those steps really depend on chosen rc rate. Smaller rc rate = smaller rate steps.  

Besides that do you really feel difference of 20-50deg/sec that much?  

Actually everything you ask is in there.  
Rc rate + rc expo added keeps the top rates, but only changes curve  
On top of that there are rates what act as super rates where you tune mid stick and than the top rates.  
I prefer the last one. It is the mid stick what is most important. That is what you tune / configure and use mostly. The top rates are not a "regular" flying scenario.  

The configurator is the limit at the moment as it is not all clear.
Eventually in betaflight configuratoe or next cleanflight configurator each axis will have 3 parameters. Rc rate, super rate and rc expo. That will make all scenarios possible for anyone.  

Quick summary: 
Rc rate: linear increase of rates  
rc expo: add expo curve to existing rate  
(sexpo) rate: Keep same liniearity on mid stick as it is now, but curve the extremes.  

Old rates:  
rc rate and rates = equal....bot liniear. How confusing was that?  

#### Another explanation from RC Slater:
Everyone should set everything that has the word Expo in it to 0. I know by default it is set to .10, but Boris himself has said it's unnecessary, and you should remove it.

ONE caveat: if you disable superexpo_rates to get the old linear control back, then you may still want to use some expo parameters to change the curve. Other than that, leave all expos at 0 when using super expo rates. (super expo is active by default)

With superexpo_rates, the rotation rate at extreme or full stick deflection is controlled by Pitch rate, roll rate, yaw rate. If you want your max flip/roll rate faster, then adjust those accordingly.

If you want to adjust how sensitive your copter feels on small corrections (around mid stick) then just adjust RC Rate. NOTE that Yaw has it's own RC rate because we sometimes want to adjust that mid stick feel on yaw separately from pitch and roll.

Example:
Stock Rates are :
Pitch Roll and Yaw Rates = .7
RC Rate = 1.0
RC Rate Yaw = 1.0

I decide I want to roughly keep maximum rotation rate the same but make mid stick more sensitive on ALL axes (including yaw)

Pitch Roll and Yaw Rates = .7
RC Rate = 1.10
RC Yaw Rate = 1.10

Now I like my mid-stick sensitivity on pitch and roll, but want more on yaw. So...

Pitch Roll and Yaw Rates = .7
RC Rate = 1.10
RC Rate Yaw = 1.20

Now I just want to increase the maximum pitch and roll rate, but leave Yaw and all my mid-stick feel the same:

Pitch Rate = .8
Roll Rate = .8
Yaw Rate = .7
RC Rate = 1.10
RC Rate Yaw = 1.20
RC Slater is online now Send a private message to RC Slater Find More Posts by RC Slater
