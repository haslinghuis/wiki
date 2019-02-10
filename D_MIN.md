# D_MIN

D_MIN provides a way to have a lower level of D in normal flight and a higher level for quick manoeuvres that might cause overshoot, like flips and rolls.  It also brings D up during prop wash.  The high level is simply your usual D number.  The low level is the  `d_min_xxx` value, which can be adjusted in the CLI or OSD.  

In the CLI, d_min settings are at the bottom of the list of profile values.

In the OSD, they are in the miscellaneous settings.

D_Cut allows the pilot to have D, and, as a result, sometimes more P, than previously, without the drawbacks of having higher D all the time.

## Background and purpose.

Lower than usual amounts of D in normal flight gives the following advantages:

- less noise, cooler motors
- cleaner motor traces at full throttle
- improved ability to react quickly to fast inputs
- D related oscillation or grinding on arming is less likely

However, low D also has disadvantages:

- greater prop wash
- greater overshoot and bounce-back
- P oscillations around fast moves, due to lack of damping when P is high

The intent of d_min is to get the best of both worlds, i.e. low D when flying smoothly and during low rate turns, higher D when needed to dampen overshoots or deal with prop wash.


## My quad is already flying great, will d_min improve it?

Maybe.  

If the quad flies well and doesn't have overshoot, oscillate, or have warm motors, there may not be much benefit from adding d_min.  You may be able to lift P a bit higher, and lift peak D a bit higher - that may give tighter handling.


## How do I disable d_min?

D_min is disabled, for a given axis, when d_min = 0, or d_min is greater than D, on that axis.  The D value is then constant all the time.


## How should I set it up for my first flights?

When pasting PIDs in, your normal D value will be retained, and the d_min values will be 20, 22 and 0 on roll, pitch and yaw.  

For first flights with d_min, I'd suggest increasing your normal D by about 10-20%, and setting the d_min values to about half that D value.


## What are the defaults?

The defaults are a guide only and will not suit all quads.

They are D of 35/38 and d_min of 20/22 on roll/pitch respectively.  


## Will adding D_min improve prop wash handling?

No.  D_Min is a way to have *less* D than usual.  Lower D typically means weaker propwash control.

The d_min algorithm does detect and respond to propwash events by bringing D back up.  But not as strongly as we would like.  Typically D only gets up by about half way to the set D value.  To get the most out of d_min, it's important to increase your D value by 15-20%.  Usually, then, the propwash handling will be about the same as before.

If propwash is your primary problem, and motors are cool, bring the d_min value up.  


## Will adding d_min improve overshoot control?

The timing of the d_min boost effect means that D is typically more effective in managing overshoot than it otherwise would be.  So even without changing your D value, d_min may improve overshoot.  

However the main advantage of d_min, if overshoot is a problem and motors are warm, is that it lets you increase D and have cooler motors at the same time.

With d_min, D itself should be increased until you have optimal overshoot control, and d_min adjusted down for motor heat control.  


## What about the 'd_min_advance' parameter?

D_min_advance speeds up of onset of the boost effect.  This can be helpful if you have overshoot with very high rate flips on very responsive quads.

Advance works by including a set point derived boost signal into the boost algorithm.  

With an advance of zero, the D boost won't start until the motors start to turn the quad, which will happen some time after the sticks are moved.  This allows FF and P to 'get started' on turning the quad early, and without any suppression by D, maximising initial turn responsiveness.  But for very responsive quads, a gyro derived boost signal can come on a bit too late.  Adding some advance will bring start boosting D as the sticks are moved, without waiting for the motors to spin.  

The advance range is 0-200.  Default of 20 does very little except with very rapid stick inputs.  A value of 100 provides significant advance.  Advance that high should not be required except possibly in very responsive quads targeting maximum turn rates in excess of 1000 deg/s.  


## What about the 'd_min_gain' parameter?

This is an advanced tuning parameter that determines how strongly D is boosted during quick stick inputs.  

The default value of 20 is ideal for the majority of quads.  Very clean quads can be OK with gain at 25.  If set higher again, the quad will not run at the minimum value much of the time, and will readily climb to the max D value.  That's actually not such a bad plan if propwash is your main concern, but its not ideal if you want a low min to control motor heat.

If the quad can be logged, the ideal gain value for general use is where the realtime D value in debugs 2 and 3 is wanting to rise up from the minimum value in normal flight all the time (not sitting exactly on the min value but going up a tiny bit at times), quickly rises to maximum D with flips and rolls, and rises to about half way up in propwash.


## How do I know what the actual value of D is that I'm getting during a flight?

1.  Use the OSD:  Enter `set debug_mode = D_MIN` in the CLI, and set your OSD to show debug2 on-screen.  The number you see is roll D times ten.  If it shows 350, you have 35 of D at that instant.  

2.  Make a log:  Enter `set debug_mode = D_MIN` in the CLI. Debug2 in blackbox explorer will show instantaneous D on roll, and debug3 shows D on pitch (note the numerical values are expressed as D*10).


## How does d_min work?
  
Propwash is characterised by gyro oscillations in the 20-60hz range. 

Quick flips and turns are associated with larger gyro changes at frequencies in the 10-20hz range.  

By applying an 80hz biquad lowpass filter to the gyro signal, these events can be detected, and higher frequency noise ignored.  Both flips and propwash will increase the boost signal.

Smoothing the absolute value of the filtered gyro signal through a 10Hz first order lowpass filter provides a way to smoothly and gently vary D when these events occur.  The 7hz lowpass delays the boost effect so that, the boost occurs more towards at the end of the input for a fast flip, rather than at the beginning.  This means the boost does not dampen the quad's initial responsiveness to commanded inputs.  

The boost strength is modulated by the gain setting.  


## Does d_min add more CPU load?

Yes, but only a little bit; one biquad filter and one PT1 filter, and some simple maths.  To find out how much extra CPU, temporarily set dterm_cut_percentage to zero and re-check CPU use in the CLI.


## Is d_min available on all F3 targets?

No.  Some F3 chips don't have enough flash space for d_min.  Boards with those chips will not show d_min options in CLI or OSD.  Special F3 builds can be made that include d_min, by re-enabling it in the relevant target definition files, but it will be necessary to exclude something else to make room.

See also:

- [the original d_cut PR7373 by ctzsnooze Jan 2019](https://github.com/betaflight/betaflight/pull/7373)
- [d_min PR7538, updating d_cut, by ctzsnooze Feb 2019](https://github.com/betaflight/betaflight/pull/7538)
- [d_min PR7559, CMS menu changes, by eTracer Feb 2019](https://github.com/betaflight/betaflight/pull/7559)
