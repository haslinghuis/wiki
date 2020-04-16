### Introduction

Dynamic Idle improves how Betaflight controls the motors at the low end of the rpm range.

By using RPM data to prevent any motor falling below a defined minimum RPM, the risk of desyncs can be minimised.  Additionally, dynamic idle improves PID controller performance at zero throttle by permitting stronger braking of motors that are in positive airflow conditions.  

NOTE 1:  Dynamic Idle requires [fully functional BiDirectional DShot telemetry](https://github.com/betaflight/betaflight/wiki/Bidirectional-DSHOT-and-RPM-Filter).  

NOTE 2: Dynamic Idle is off by default.  To enable it, set `idle_min_rpm` to a suitable value in the CLI.  20 is a suitable starting point.  Please read the tuning section, below.

NOTE 3: Dynamic Idle must not be used with 3D mode.

NOTE 4: Transient throttle limit should be disabled while using Dynamic Idle (`set_transient_throttle_limit = 0`)


### How does Dynamic Idle work?

#### Let's first explain how idle is managed without dynamic idle.  

Without dynamic idle, the lower limit of motor drive, under all conditions, is set by the `dshot_idle_value`. This defaults to 5.5%.   When throttle stick is at zero, the motors get sent 5.5% motor drive.  The PIDs cannot reduce motor drive above 5.5%, either. 

When we decelerate suddenly, the motors may get airflow pushing up into the props from underneath.  This slows them down a lot, and they may either turn so slowly that they can't quickly start up again when needed, or stop spinning completely, or even spin backwards.  This can lead to poor handling or total loss of control (motor desync), usually at the end of fast flips or rolls.  To avoid these issues, the `dshot_idle_value` must be set relatively high.  This limits hang time and doesn't allow strong motor braking.

Conversely, if we want to quickly stop a fast roll move, we need to slow down the driving motors.  When we tell them to stop, they are moving quickly forward in the air.  They  have strongly positive inflow, and the airflow pushing into them keeps them spinning faster than they should be.  The PIDs would really like to send zero motor drive to them - maximal braking - but cannot send less than 5.5%, even though they are in no danger of stopping.  Because they can't slow down as much as we would like, our ability to stop the turn is not as strong as it could be.

Finally, lets consider an inverted drop, with a yaw while inverted.  While dropping cleanly, the motors will be at 5.5% motor drive, pushing downwards.  We might like to lower this value to improve 'hang time', but are limited by needing a high enough number to prevent desyncs.  If do a yaw while inverted, or keep the quad stable, the PIDs can only make the motors go faster, and speeding up the motors while inverted really limits hang time.

#### How does dynamic idle change things?

With dynamic idle enabled, the PIDs are allowed to send zero drive to the motors, so long as the motors do not spin too slowly.  The actual RPM is continually monitored using Bidirectional DShot telemetry.  Idle motor drive is then dynamically adjusted so that the slowest motor can't fall below the configured minimum RPM - even if its drive signal is zero.

Dynamic idle therefore can greatly reduce the chance of a desync.

Because the motors won't slow down under strong reverse airflow conditions, they can start up quickly.  This provides improved zero throttle turn responsiveness, improving stability against cross-axis wobble and improving response time when stopping a flip quickly, and improving zero throttle stability.

Under strongly positive inflow conditions, where the motors will be spinning faster than expected, they can now be braked much harder, since the PIDs can send as little as zero drive to slow them down, instead of the mandatory 5.5%..  This is beneficial when quickly stopping a fast flip or roll, or when performing tricks during inverted drops.  

These are the three key elements to dynamic idle:

1.  We get the actual motor RPM via bidirectional DShot telemetry, and use this to prevent any motor falling below the set `idle_min_rpm` value.  

2.  We allow the PIDs to send motor drive to zero, provided that RPM is OK

3.  The `dshot_idle_value` becomes our minimum throttle value.

At simple idle, with minimal PID activity, the normal DShot idle percentage (`dshot_idle_value`) is sent to the motors, and they spin at whatever RPM that value would generate, or the minimum RPM set by `idle_min_rpm`.  

Under all conditions, the dynamic idle minimum value becomes the effective minimum idle RPM, a kind of 'safety net' under which RPM should not fall.  

### Tuning.

`idle_min_rpm` is the key setting that enables Dynamic Idle.

The ideal setting for `idle_min_rpm` is about 10% less less than the RPM as shown in the motors tab at idle.  If your idle was 5%, you'd set the motor to 1050 in motors tab, and check the RPM there.  If it was, say 2,200 rpm, then you'd use 20 as your `idle_min_rpm`.

A value of 20 corresponds to 2,000 RPM and is a reasonable starting value for most quads.

Smaller quads rev faster, and a higher `idle_min_rpm` value, eg 25, may be more appropriate for a 2.5".

if you already are running a known good DShot idle value, use that and an `idle_min_rpm` value set as above.

#### Can I set it up to improve turn responsiveness even more?

Not really.  The defaults, configured as above, provide most of the available benefits 'out of the box'.  A slightly higher than usual `dshot_idle_value` will keep the motors spinning more quickly when we cut the throttle, leading to more rapid spool up when needed, but with dynamic idle and an `idle_min_rpm` value set as above, the motor drive can still fall to zero if the PIDs need it.  Too high and it will feel floaty.

#### I'm after lots of inverted hang time

Dynamic idle can allow longer inverted hang time, but you must aim for the lowest possible values of both DShot idle percentage and `idle_min_rpm`.  Typically you should set the `idle_min_rpm` set very close to or the same as your idling RPM from the motors tab.  You would cut both values, stepwise, going as low as you can with both.

For example, if you were running DShot idle of 5% and that gave you 2000 rpm, you could try DShot idle of 4% and `idle_min_rpm` of 15.  If that was OK, then 3.5% and 13.  If hard flip stops get ragged, or you start tumbling out of the sky under reverse airflow due to a desync, that's too low.

Low idle settings that maximise inverted time will not be good for turn performance.


#### Is this useful for whoops?

Yes, for sure.  Whoops have low authority at idle that is often improved significantly with Thrust Linear.  This will result in active PIDs at zero and hover throttle, which can cause low RPM states. 

Whoops may require higher than default idle values to keep their motors spinning reliably.


#### I'm a very high turn rate Acro LOS pilot

When targeting very high maximum turn rates - 1800 deg/s for example - desyncs can be more of a problem.  For maximum acrobatic and hang performance, the `dshot_idle_value` can be lower than usual, and the `idle_min_rpm` value high enough to stop desyncs.  

Note that if the rpm from `idle_min_rpm` is higher than that of DShot idle, hovers may not be smooth, since the dynamic idle controller may hunt around a little.


### The technical stuff

Dynamic idle adjustments are not instantaneous.  The correction factor uses a special PID controller where the adjustment is provided that is proportional to the difference between the needed and actual motor accelerations.  The delays and feedback mechanisms result in some potential for instability when extreme values are used.  

The stability of the dynamic idle controller can be tested by hovering with very low dynamic idle.  The quad will rely entirely on dynamic idle to maintain stable hover RPM.  On unusual builds, it might be unstable so take care.  You would do this kind of testing only to check that the dynamic idle system was working well.  Usually it is not needed.

The secondary settings are best left at defaults.

`idle_adjustment_speed` is a gain multiplier on the crude difference between actual and target rpm.  It affects magnitude of response, more than speed.

The gain of the controller is set by `idle_p`.  Higher values will cause corrections to be faster.  If too high you may get oscillations while hovering only on Dynamic idle.  If too low, the responsiveness will be slow, potentially lagging too much and resulting in desyncs.

`idle_pid_limit` is a constraint on the maximum amount of allowed error correction.  If set too low, reverse airflow may still cause a desync because the amount of correction may be inadequate.  

`idle_max_increase` is a limit on the maximum allowed percentage increase in idle above zero, expressed as percent *10.  Default of 150 means 15% motor drive is the maximum increase that can be applied to prevent a desync.  Higher values run the risk of driving the other motors very hard in the event of a prolonged desync.

The Dynamic Idle debug returns:

    0 : motorRangeMinIncrease * 1000
    1 : targetRpsChangeRate (simple RPM error * idle_adjustment_speed)
    2 : error (amount of error to fix)
    3 : minRps (lowest current motor rpm, in revolutions per second)

