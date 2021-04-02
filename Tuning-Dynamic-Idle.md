### Introduction

Dynamic Idle improves how Betaflight controls the motors at the low end of the rpm range.

By using RPM data to prevent any motor falling below a defined minimum RPM, the risk of desyncs can be minimised.  This allows lower overall idle settings, increasing straight line braking and hang time.  Additionally, dynamic idle improves PID controller performance at zero throttle by permitting stronger braking of motors that are in positive airflow conditions.  

NOTE 1:  Dynamic Idle requires [fully functional BiDirectional DShot telemetry](https://github.com/betaflight/betaflight/wiki/Bidirectional-DSHOT-and-RPM-Filter).  

NOTE 2: **Dynamic Idle is off by default**.  To enable it, set `dyn_idle_min_rpm` to a suitable value in the CLI.  For typical 5" quadcopters 20 is a suitable starting point.  Please read the tuning section, below.

NOTE 3: Dynamic Idle must not be used with 3D mode.

NOTE 4: **Transient throttle limit should be disabled while using Dynamic Idle** (`set transient_throttle_limit = 0`)


### How does Dynamic Idle work?

#### Let's first explain how idle is managed without dynamic idle.  

Without dynamic idle, the lower limit of motor drive, under all conditions, is set by the `dshot_idle_value`. This defaults to 5.5%.   When throttle stick is at zero, the motors get 5.5% motor drive.  The PIDs cannot reduce motor drive below 5.5%, either. 

Imagine we are in a flat drop, or have done a quick 180 degree reversal from high forward speed.  In both these situations, airflow is pushing up into the props from below the quad (negative inflow).  This can slow them down a lot.  They may either turn so slowly that they can't start quickly when needed, or stop spinning completely, or even spin backwards.  This can lead to poor handling or total loss of control (motor desync), often most obvious at the end of fast flips or rolls.  To avoid these issues, we usually need to set the `dshot_idle_value` relatively high.  This limits inverted hang time, makes the quad more floaty, and doesn't allow strong motor braking.

If we want to quickly stop a fast roll move, we need to slow down the driving motors.  When we tell them to stop, they are moving quickly forward in the air.  They have strongly positive airflow which keeps them spinning faster than they need to be.  The PIDs would really like to send zero motor drive to them - maximal braking - but cannot send less than 5.5%, even though they are in no danger of stopping.  Because they can't be slowed down as much as they could, our ability to stop the turn is not as good as it could be.

Finally, lets consider an inverted drop, with a yaw while inverted.  While dropping cleanly, the motors will be at 5.5% motor drive, pushing downwards.  Air is flowing into the props as we drop, and likely they are spinning faster than is needed.  We would like them to spin more slowly, or resist spinning harder, to improve 'hang time' - but we can't, because DShot idle is a high enough number to prevent desyncs.  Additionally, if we try to yaw while inverted, the PIDs can only make the motors go faster than idle, and speeding up the motors while inverted really limits hang time.  

#### How does dynamic idle change things?

With dynamic idle enabled, the PIDs are allowed to send zero drive to the motors, so long as they don't spin too slowly.  The actual RPM is continually monitored using Bidirectional DShot telemetry.  Idle motor drive is dynamically adjusted to keep the slowest motor above the configured minimum RPM - even if its drive signal is zero.

Hence dynamic idle will greatly reduce the chance of a desync, and allow lower overall idle settings.

Because the motors won't slow down as much under strong reverse airflow conditions, they can start up quickly.  This provides improved zero throttle turn responsiveness, greater stability against cross-axis wobble when stopping a flip quickly, and improved zero throttle stability in flat drops.

Under strongly positive inflow conditions, where the motors will be spinning faster than expected, they can now be braked much harder, since the PIDs can send as little as zero drive to slow them down, instead of the mandatory 5.5%.  This is beneficial when quickly stopping a fast flip or roll, or when performing tricks during inverted drops.  

These are the three key elements to dynamic idle:

1.  We get the actual motor RPM via bidirectional DShot telemetry, and use this to prevent any motor falling below the set `dyn_idle_min_rpm` value.  

2.  We allow the PIDs to send motor drive to zero, provided that RPM is OK

3.  The `dshot_idle_value` becomes our minimum throttle value.

The dynamic idle minimum value becomes the effective minimum idle RPM, a kind of 'safety net' under which RPM should not fall.  

## Setup - enabling dynamic idle

`dyn_idle_min_rpm` must be above zero to enable Dynamic Idle.

The ideal setting for `dyn_idle_min_rpm` is about 20% less than the RPM shown in the motors tab at idle.  

- Take props off !!
- Check your Dshot Idle value in the configuration page of the configurator
- Go to the motors tab
- Re-check that you really did take props off !! :-)
- Connect a Lipo at around 3.8 - 3.9V
- Enable the motors with the switch
- Use the master slider to adjust motor drive to your idle value, which is 10XX where XX is your idle value.
- For example, if your Dshot Idle value is 5%, set the motor drive to 1050
- check the RPM number under the bar graph, take two zeroes off, and take off 20% (or multiply by 0.8)
- set your dynamic idle minimum RPM value to that number.

For example, if the idle RPM at 1050 was say 2,000, take two zeros off to get 20, then take off 20% (or multiply by 0.8) to get 16; set your dynamic idle minimum (`dyn_idle_min_rpm` in CLI) to 16.

Always measure rpm properly using the motors tab.  Do not guess the numbers.  

You may arm with the Tx in the motors tab (after setting the enable switch) to check that the idle RPM in real world is about the same as it is when running directly from the motors tab slider.  You may notice that noise from the motors will cause some variability, but the RPM numbers should be about the same.  Don't lift the throttle at all after arming!

Note that after configuring the minimum idle RPM as above, the actual idle rpm in the motors tab does not change.  

If you already are running a known good DShot idle value, start off with that, and set the `dyn_idle_min_rpm` value set 20% lower as described above.  

**NOTE: If you want to change your overall idle value, you must change your DShot `Motor Idle Throttle Value`, and then re-adjust your `dyn_idle_min_rpm` value, as above.**

There is no need to change anything if you enable `motor_output_limit` at less than 100, since it does not change the idle rpm, attenuating motor drive above the idle value only.


## Tuning

Most of the benefits of dynamic idle happen 'out of the box' as soon as dynamic idle is enabled at the correct rpm value.  Turn responsiveness, drop stability and hang time should improve immediately.

Higher idle values (both increased together) will keep the motors spinning more quickly when the throttle is cut, or in strongly positive inflow states.  This will lead to more rapid spool up when needed, better zero throttle stability, and crisper flips - at the cost of less effective braking and a more 'floaty' feel, and reduced inverted hang time.   

Lower idle values (both reduced together) will lead to improved inverted hang time, at the cost of reduced stability when chopping throttle hard after a punch, when blipping up from idle, and any time you are at idle, eg mid-flip, flat drops, etc.  Because the motors will idle more slowly, they may have difficulty re-starting after being commanded to idle, eg at the end of a flip, causing instability and bounce back.  

Every quad is different, and the purpose we put them to varies a great deal.  Unless your requirements or build are unusual, the default idle value is usually quite good.  

Low authority quads (larger props, weaker motors, ducted cine quads, endurance quads) will typically do better with higher idle values.

High authority quads often fly best with idle values around 4-4.5%.


#### I'm after lots of inverted hang time

To get longer inverted hang time, go for the lowest possible values of both DShot idle percentage and `dyn_idle_min_rpm`.  Keep `dyn_idle_min_rpm` adjusted relative to DShot as described above (20% lower than the idle rpm).  

REMEMBER: both values must be reduced!  

You can go as low as you like, eventually you will get annoying instability with throttle chops, or desyncs.

If you were running DShot idle of 5% and `dyn_idle_min_rpm` of 20 was 20% below rpm at 5%, you could try DShot idle of 4% and `dyn_idle_min_rpm` of 16.  If that was OK, then 3.5% and 14, even 3% and 12. 

When hard flip stops or throttle chops get ragged, or you start tumbling out of the sky under reverse airflow due to a desync, that's too low.  You can typically get well below the default DShot idle value.  

Really low idle settings may maximise inverted time but will adversely affect stability as described above.


#### Is this useful for whoops?

Yes, for sure.  Whoops have low authority at idle that is often improved significantly with the combination of Thrust Linear and Dynamic Idle.  

Whoops may require higher than default idle values to keep their motors spinning reliably.


#### I'm a very high turn rate Acro LOS pilot

When targeting very high maximum turn rates - 1800 deg/s for example - desyncs can be more of a problem than for quads with lower target rates.  

For maximum acrobatic and hang performance in a high power to weight LOS quad, the goal would be the lowest idle values that didn't cause desyncs or other adverse effects like excessively slow startup after flips.  

Note that if the rpm from `dyn_idle_min_rpm` is higher than that of DShot idle, hovers may not be smooth, since the dynamic idle controller may hunt around a little, but desyncs are less likely.  

#### I'm a race pilot

At present we have no hard data on how best to configure dynamic idle for racing.

Racers typically use low throttle very little of the time, so you might wonder why you'd bother with something that only influences idle behaviour.  

There are two reasons.

First, braking.  Sure we want to go fast, but we also want to slow down.  If we can set idle lower than usual, we can brake harder.  Dynamic idle lets us do that.  We can go full throttle, and when we cut throttle, we will bleed off speed more quickly, making the next turn easier.  We will also be able to drop into dive gates more aggressively on cutting throttle.  

Second, turn performance high speed.  Typically at high speed we have strong positive inflow. We can turn faster with dynamic idle active because under positive inflow conditions it can brake motors down to zero drive, helping turn in more quickly.  

When flying race courses that involve high speed entries to maneuvers such as split-S or tight hairpins, dynamic idle may allow more consistent 'reverse throw' and improved control/propwash management, allowing more accurate rhythm-based turn entry and vertical maneuvers (e.g. repetitive dive/antigravity gates or inverted dive entries).

Optimal dynamic idle for experienced racers, who keep throttle on most of the time, may be with slightly reduced idle values.  Something like your DShot idle reduced to say 4 - 4.5%, and `dyn_idle_min_rpm` 20% below that, maybe.  The downside of lower idle values is that if you do make a mistake and do a zero throttle reversal, it may be uglier.  


### The technical stuff

Dynamic idle adjustments are not instantaneous.  The correction factor uses a special PID controller where the adjustment is proportional to the difference between the needed and actual motor accelerations.  Feedback delays have some potential for instability when extreme values are used.  

The stability of the dynamic idle controller can be tested by hovering with very low dynamic idle.  The quad will rely entirely on dynamic idle to maintain stable hover RPM.  On unusual builds, it might be unstable so take care.  You would do this kind of testing only to check that the dynamic idle system was working well.  Usually it is not needed.

The secondary settings are best left at defaults.

`idle_adjustment_speed` is a gain multiplier on the crude difference between actual and target rpm.  Default is 50. 

`idle_p` sets the gain of the controller.  Higher values will cause corrections to be more aggressive. 

These two parameters should not need adjusting in most cases.  To confirm that they are appropriate for your quad, enable the RPM debug, fly straight vertically up at speed, cut throttle quickly to zero, and drop straight flat down under zero throttle.  If the motor traces and rpm traces are smooth, all is good.  If they are wobbling, the values may need to be tweaked a bit.  My race quads work very well with `idle_p` a bit higher, at 80, but increasing `idle_adjustment_speed` as well made the rpm traces a bit sawtoothed.

`idle_pid_limit` constrains the maximum allowed error correction.  If set too low, reverse airflow may still cause a desync because the amount of correction may be inadequate.  

`idle_max_increase` is a limit on the maximum allowed percentage increase in idle above zero, expressed as percent *10.  Default of 150 means 15% motor drive is the maximum increase that can be applied to prevent a desync.  Higher values run the risk of driving the other motors very hard in the event of a prolonged desync.

The Dynamic Idle debug returns:

    0 : motorRangeMinIncrease * 1000
    1 : targetRpsChangeRate (simple RPM error * idle_adjustment_speed)
    2 : error (amount of error to fix)
    3 : minRps (lowest current motor rpm, in revolutions per second)