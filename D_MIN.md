When Dmin enabled (via the switch in the Configurator PID page), you can set independent Dmin and Dmax values, to have different amounts of D depending on what the quad is doing at the time.

This allows us to have a lower amount of D in normal flight - the Dmin value - and a higher level for quick manoeuvres that might cause overshoot, like flips or rolls, and during propwash type events.

When D_min is enabled, the normal D value is renamed as the Dmax value, and a Dmin column appears.  The Dmin value becomes the 'normal' value that is active in normal straight flight, and gentle turns.  Dmax becomes a 'high' value that is only active during quick moves or during propwash events.

Dmin values are, like D/Dmax values, saved per-profile.

In the OSD, they are in the PID tuning settings.


## Application

D_min allows the pilot to have low D most of the time, leading to reduced D delay and cooler motors, while allowing more D and well controlled P during quick moves and propwash.

Freestylers would typically set Dmin to close to the 'normal' or default D value,  and bring Dmax say 20-30% higher, to help control propwash and overshoot.

Racers would typically leave the Dmin to Dmax proportion as per default.

Cinematic HD pilots need high P and high D values all the time, so Dmin should be disabled.  7" or larger cinematic quads, will usually have least background wobble while cruising with a relatively high P, with Dmin off, with D about 10% higher than P, and with stronger overall D filtering (D filter sliders hard left).

The primary driver to increase Dmin towards Dmax is gyro movement.  Stronger gyro movements, including propwash shaking, will trigger an increased in D.  

A secondary element (Dmin advance) can optionally be added to boost D a bit earlier by driving it from setpoint.  


## Background

Lower than usual amounts of D in normal flight gives the following advantages:

- less noise, cooler motors
- cleaner motor traces at full throttle
- faster gyro responses to quick inputs
- D related oscillation or grinding on arming is less likely

However, low D also has disadvantages:

- greater prop wash
- greater overshoot and bounce-back
- P oscillations around fast moves, due to lack of damping when P is high
- low level slow oscillations in smooth forward flight

The intent of d_min is to get the best of both worlds, i.e. low D when flying smoothly and during low rate turns, higher D when needed to dampen overshoots or deal with prop wash.


## My quad is already flying great, will d_min improve it?

Maybe.  

If the quad flies well, doesn't have overshoot, doesn't oscillate, doesn't have excessively warm motors, and doesn't have much propwash, there may not be much benefit from enabling d_min.

If the quad has warm to hot motors, and in particular if it is a racer, Dmin at a value lower than usual D is typically very helpful, improving turn-in responsiveness, bent prop tolerance, and reducing motor heat.

For freestyle purposes, if the quad is flying well and has cool motors, but has propwash, setting Dmin to your 'normal' D value and using Dmax as a 'boost' for propwash, by setting Dmax 20-40% higher than Dmin may help attenuate the propwash. 

Sometimes quads that have an 'one the edge' tune, with relatively high P, will get noticeable and audible P oscillation during tight power-on turns.  Using Dmax as a D boost will give you more D at these times which can help control that P oscillation better, allowing higher overall P values.


## How do I disable d_min?

Switch it off in the Configurator.

D_min can also be disabled, in the CLI, by setting d_min = 0, or if d_min is misconfigured to be greater than D.  

When disabled, the users Dmax value becomes the D value, and D is constantly at that value, all the time.


## How should I set it up for my first flights?

The default Dmin values will be 23, 25 and 0 on roll, pitch and yaw.  These are good, safe, starting points, for most quads.  

For high-performance builds, and all new builds, the first flights should be done with the master P and D slider left a few clicks, just in case the PIDs are a bit high.

When tuning P and D with Dmin active, be sure to appreciate that the Dmax value is active during the faster periods of flips and rolls, and the Dmin value is active during steady flight and for smaller moves. 

Some people advocate tuning P and D with Dmin off.  The idea is to determine the P value that causes oscillation, and then the amount of D required to control it.

The pilot should first set D to about a third of P, push both upwards stepwise until P oscillation occurs, eg during tight fast turns, then back off both P and D until there is only a hint of P oscillation remaining.  Then D can be brought up to a value that controls it, which usually would be about a number close to or just below the P value.  Freestylers should use this value for Dmin.  They may then be able to drive P higher until P oscillations just re-occur, and use Dmax at a higher value to control it.  This should give optimal propwash handling as well.

## Will adding D_min improve prop wash handling?

If the Dmin value is set to your optimal D value, and Dmax is set to a higher value, this is likely to improve propwash.  

If the Dmin value is set below the optimal D value, to keep motors cool / make the quad more responsive / improve bent prop tolerance, eg for racing, then propwash will likely be worse.

During propwash events, D typically climbs only about half-way to the Dmax value.  The 'sensitivity' of the Dmin code to propwash is affected by the `d_min_boost_gain` parameter.  Higher gains will push D up more quickly and more aggressively.  The default is conservative.  It can be set to 30-35 on most reasonable quads, and to 40-45 on really clean freestyle builds.

If propwash is the primary problem, and motors are cool, bring the d_max value up, and the `d_min_boost_gain` up. 

For fine tuning of that kind, it's best to make a blackbox log with the debug mode set to D_MIN.  This will show the amount of D you're actually getting at different times in the flight.


## Will adding d_min improve overshoot control?

Yes.  The timing of the d_min boost effect typically improves overshoot, since D will peaks around the time the gyro-detected turn rate is highest, which is when overshoot is most likely. 


## What about the 'd_min_advance' parameter?

`D_min_advance` speeds up of onset of the boost effect, by boosting D when setpoint changes (as well as when gyro changes).  Changes in setpoint happen immediately you move the sticks, before the quad starts to move, and before the gyro detects any change at all.

The default is 20, which is very little.

Zero advance is best for racers, and probably for most quads.  

Stronger advance (up to 100, say) will push D up very early and that may improve overshoot control on some freestyle setups and in LOS quads that target extremely high maximum roll rates and are very susceptible to overshoot.

Because strong advance adds delay, try to use the lowest value possible.  Always try it at zero, and if there is no apparent change in the handling of the quad, use zero.  

With an advance of zero, the D boost won't start until the motors start to turn the quad, which will happen some time after the sticks are moved.  This allows FF and P to 'get started' on turning the quad early, and without any suppression by D, maximising initial turn responsiveness.  But for very responsive quads, a gyro derived boost signal can come on a bit too late.  Adding some advance will bring start boosting D as the sticks are moved, without waiting for the motors to spin.   


## What about the 'd_min_boost_gain' parameter?

`d_min_boost_gain` determines how quickly, and strongly, D is boosted in response to flips and rolls and propwash.  

The default value of 20 is safe for the majority of quads.  30-35 is a good value for a clean freestyle quad.  Some extremely noise free quads can be OK with gain at 40-45.  

Dmin gain at the default of 20 is optimal for racers.  It  will keep D at the Dmin value most of the time, only coming up for stronger moves to prevent overshoot.  It will not help much for propwash like this but gives the racer all the responsiveness and overshoot control benefits.

Dmin gain of 35 is good for freestyle generally.  Higher values to about 40-45 may be used on very clean quads but are best checked by logging with the debug set to D_MIN to check exactly what is happening.

The ideal gain value should result in little or no boost in gentle forward flight but a quick and significant boost effect with propwash.

Note that high Dmin gain  will make the Dmin boost come on whenever there is fast quad movement or shaking.  A bent prop, it will cause D to go to the Dmax value, and if Dmax is high, you will get a lot of D mediated motor heating.  Hence it is really important to not use high gain with high Dmax value unless the build is clean and shake-free.  

High Dmin gains should only be used for really clean builds that won't be flown with bad props.  

If the quad can be logged, the ideal gain value for general use is where the realtime D value in debugs 2 and 3 is wanting to rise up from the minimum value in normal flight all the time (not sitting exactly on the min value but going up a tiny bit at times), quickly rises to maximum D with flips and rolls, and rises to about half way up in propwash.


## How do I know what the actual value of D is that I'm getting during a flight?

1.  Use the OSD:  Enter `set debug_mode = D_MIN` in the CLI, and set your OSD to show debug2 on-screen.  The number you see is roll D times ten.  If it shows 350, you have 35 of D at that instant.  

2.  Make a log:  Enter `set debug_mode = D_MIN` in the CLI. Debug2 in blackbox explorer will show instantaneous D on roll, and debug3 shows D on pitch (note the numerical values are expressed as D*10).

Debug 2 shows D on roll, Debug 3 shows D on pitch.  Both are before TPA attenuation.  
Debug 0 shows the gyro contribution, and Debug 1 the setpoint (advance) contribution, to the boosting effect.

Note from @docteh: The debug OSD element in 4.0 and 4.1 shows debug 0 1 2 3 on one big line. You also need to have d_min enabled for values to show up here, as the element needs those values to show anything. This "debug2" stuff needs a bit of work.

## How does d_min work?
  
Propwash is characterised by gyro oscillations in the 20-60hz range. 

Quick flips and turns are associated with larger gyro changes at frequencies in the 10-20hz range.  

By applying an 80hz biquad lowpass filter to the gyro signal, these events can be detected, and higher frequency noise ignored.  Both flips and propwash will increase the boost signal.

Smoothing the absolute value of the filtered gyro signal through a 10Hz first order lowpass filter provides a way to smoothly and gently vary D when these events occur.  The 10hz lowpass delays the boost effect so that, the boost occurs more towards at the end of the input for a fast flip, rather than at the beginning.  This means the boost does not dampen the quad's initial responsiveness to commanded inputs.  

The boost strength is modulated by the gain setting.  


## Does d_min add more CPU load?

Yes, but only a tiny bit; one biquad filter and one PT1 filter, and some simple maths.  To find out how much extra CPU, temporarily set d_min to zero on all axes and re-check CPU use in the CLI.


## Is d_min available on all F3 targets?

No.  Some F3 chips don't have enough flash space for d_min.  Boards with those chips will not show d_min options in CLI or OSD.  Special F3 builds can be made that include d_min, by re-enabling it in the relevant target definition files, but it will be necessary to exclude something else to make room.

See also:

- [the original d_cut PR7373 by ctzsnooze Jan 2019](https://github.com/betaflight/betaflight/pull/7373)
- [d_min PR7538, updating d_cut to d_min, by ctzsnooze Feb 2019](https://github.com/betaflight/betaflight/pull/7538)
- [d_min PR7559, CMS menu changes, by eTracer Feb 2019](https://github.com/betaflight/betaflight/pull/7559)



____________________________________

