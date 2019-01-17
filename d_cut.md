# D_Cut Wiki notes

D_Cut attenuates the effective amount of D in normal flight, and brings D back up to normal for fast flips and other quick manoeuvres that might cause overshoot.  It also brings D up during prop wash, the amount of increase depending on the amount of propwash.  The amount of cut can be adjusted in the CLI or OSD.

D_Cut allows more D than usual (and sometimes more P) in quads that were previously limited in how much D they could run.


##Background and purpose.

Less D in normal flight provides the following advantages:

- less D induced noise amplification and feedback, resulting in cooler motors and cleaner motor traces
- cleaner motor traces at full throttle mean more power and speed
- the quad's ability to react quickly to fast inputs is improved, making it feel snappier
- D related oscillation or grinding is less likely

However, if D is low all the time, low D may result in the following disadvantages:

- greater prop wash
- greater overshoot and bounce-back
- P oscillations around fast moves, due to lack of damping when P is high

The intent of D_Cut is to provide a means where the quad can be tuned to get the best of both worlds, i.e. low D when flying smoothly or during low rate turns, and higher D when needed to dampen overshoots or deal with prop wash.


## My quad is already flying great, will D_Cut improve it?

Probably not.  If you don't have overshoot, and motors are cool, and you've never felt that more D would be useful, you don't need D_Cut.


## How do I disable D_cut?

In the CLI or OSD, set dterm_cut_percent = 0.  This stops all computations associated with D_Cut.  To keep the computations running but effectively do nothing, set dterm_cut_percent = 1.  


## How do I get back to default cut percentage?

Default is 65% cut, meaning that only ⅓ of your set D is active in normal flight.  


## Why so much cut by default?

When D_Cut is available, the default D is 40 rather than 30 (on roll), and D_Cut of 65% applied.  The result is D of only 14 at idle and in steady flight, rising close to 30 in propwash, and 40 for flips.  

if you are intending to use the default cut amount over a previously tuned quad, D should be increased by about a third (or more).


## Will adding D_Cut improve prop wash handling, or improve overshoot, if I keep my D value unchanged?

No.  Propwash won't be improved if cut is simply added to pre-existing D values, because the quad will get less D than usual during propwash events.

Overshoot will be just as good, though.

The main benefit of adding cut without increasing D, is that motors will be cooler and smoother running than they previously would have been.


## How do I use D_Cut to improve prop wash or overshoot after flips?

In principle, you configure D_Cut to boost D during prop wash or flips.

Let's say a quad with D at 40 has a little bit of overshoot and some prop wash, and motors are warm but OK.  Previous attempts to increase D have caused problems with motor temps, or sluggishness, or not tolerating bent props so well.

If D_Cut was enabled at 33%, and D increased to 60, the quad will run with 40 of D nearly all the time, like before.  Motor temps and stick feel will likely be exactly the same, since D is the same as usual.  However, during prop wash, D will rise, maybe to 50 or 55, and this extra D may help control propwash.  

Overshoot after flips or rolls will be better controlled, leading to smoother and better controlled handling after sudden roll or pitch stops, since you'll get 60 of D at the end of the your sharp inputs, which will dampen and smooth out the tendency to overshoot more strongly than 40 would have.


## What should I set the dterm_cut_percentage to?

It depends whether your goal is to cut D to control motor temps in normal flight, or boost D to improve propwash or flip control.  The right amount of D for a given quad can only determined by you, the pilot, as part of your tuning process, and is influenced by many things.  The following figures may help explain a range of ways to use D_Cut:

- With a D value of 40, a cut of 25% will mean that D runs at 30 most of the time.  This might reduce motor temps quite a lot without changing handling all that much.

- With a D value of 40, and set cut to 50%, D will run at 20 most of the time.  This might markedly reduce motor temps, keep flips much the same, but be associated with more propwash.

- With a D value of 60, and cut of 33%, D will run at 40 most of the time.  If 40 was OK before, this may improve propwash and flip control without making the motors any hotter.

- With a D value of 80, and set cut to 50%, D will run at 40 most of the time.  That D could rise as high as 80 might be too much for the average 5" quad, and is likely to worsen propwash and cause D related oscillation during flips.  However these values may be quite good for low authority quads like whoops where a lot of P and D are needed.


## What about the 'dterm_cut_gain' parameter?

This is an advanced tuning parameter that determines how strongly D is amplified during quick stick inputs.  

The default value of 15 is ideal for the vast majority of quads.  

Cutting gain to around 10 will typically result in D not getting back up the full set value during flips and rolls, and won't rise much at all with propwash.  The benefit of D_Cut won't be so obvious because the 'boost' back to normal after flips and rolls will be later more brief, and weaker.  People with very aggressive flying styles may cut gain to 12-13.

Increasing gain to around 20 will typically cause D to come back up with to less strong inputs, and to fully rise up in propwash, and come back up to normal earlier, and last longer, in flips and rolls.  This can be OK for smooth freestyle flying.  


## How do I know what the actual value of D is that I'm getting during a flight?

1.  Use the OSD:  `set debug_mode = D_CUT` in the CLI, and set your OSD to show debug2 on-screen.  The number you see is roll D times ten.  If it shows 350, you have 35 of D at that instant.  

2.  Make a log:  `set debug_mode = D_CUT` in the CLI, and display debug2 for instantaneous D on roll, or debug2 for D on pitch.


## How can I tune the gain parameter?

Optimal tuning requires a log with `set debug_mode = D_CUT`.  D_Cut works best if the gain hits 100% by the time the D PID trace peaks during fast inputs.  In other words, the set D value should be achieved at or just before the peak in D itself.


## How does D_Cut know when to add or cut D??
  
Propwash is characterised by gyro oscillations in the 20-60hz range. Quick flips and turns are associated with larger movements at frequencies in the 10-30hz range.  By applying a 40hz biquad lowpass filter (dterm_cut_range_hz) to the gyro signal, these events can be detected, and higher frequency noise ignored. Smoothing the absolute value of that signal with a 7Hz first order lowpass filter (dterm_cut_lowpass_hz) provides a way to smoothly and gently vary D when these events occur.  The 7hz lowpass delays the boost effect so that, with flips, the boost occurs primarily at the end of a fast flip, rather than at the beginning, so as to not delay the movements commanded by the pilot, but to smooth out overshoot that might happen at the end.  A 7Hz first order lowpass has a time constant of about 23ms, so the actual peak of the boost signal is quite delayed.  The gain is a simple multiplier of the boost effect.  It needs to be such that we are not 'only just' getting D to normal at the very peak of the boost signal, but before and after the peak.  Once gain is set correctly, the default delay provides maximum D at the right time for the majority of quads.  The gain is lowpass compensated so that changes to lowpass should not require a change in gain.


## Do I need to tune the dterm_cut_lowpass_hz value?

Probably not.  The default value is very good for the majority of quads.

The dterm_cut_lowpass_hz value determines the amount of smoothing and delay of the boosting effect.  Higher values bring the boost on sooner, cause it to last a shorter time, and make it less stable.  Lower values make the boost come on more gradually, and last longer.

If the quad has relatively low authority, making it slow to roll or flip, a lower dterm_cut_lowpass_hz value, perhaps 5hz, with 1-2 points more gain, may work better.

If the quad is super responsive, its dterm_cut_lowpass_hz value could be increased from 7hz to say 10hz, so that the effect peaks more quickly.  


## What about the dterm_cut_range_hz value?

Ignore it. The current default value is optimal for the majority of quads.

The dterm_cut_range_hz value determines the range of frequencies that will be considered as worthy of being boosted.   

A higher value will make DCut more responsive to higher frequencies in propwash and some noise signals.  Currently it is difficult for the motors to respond fast enough for a D boost to be useful at higher frequencies than 40hz.  However a really responsive quad, with fast motor responses and propwash at higher frequencies than normal, may benefit from increasing this to 50 or 60hz.  Too high a frequency risks boosting D with signals that will only end up making the motors hot.  I would not recommend increasing this value unless your really wanted to tinker a bit.

A lower value will make DCut less responsive to higher frequencies.  This may be good for low authority machines like whoops or large prop quads, where the motors just can't respond to frequencies above say 30hz, or where the propwash frequency is quite low.  


## How much additional CPU load does D_Cut add?

Very little; one biquad filter and one PT1 filter, and some simple maths.


## Is D_Cut available on all F3 targets?

No.  Some F3 chips don't have enough flash space for D_Cut.  Boards with those chips will not show dterm_cut options in CLI or OSD, and it will not run.  Special F3 builds can be made that include D_Cut, by re-enabling it in the relevant target files, but it will be necessary to exclude something else to make room.

See also : [Original PR7373 by ctzsnooze Jan 2019](https://github.com/betaflight/betaflight/pull/7373)

