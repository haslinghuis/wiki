### Introduction

Dynamic Idle improves how Betaflight controls the motors at the low end of the rpm range.

By using RPM data to prevent any motor falling below a defined minimum RPM, the risk of desyncs can be minimised.  This allows lower overall idle settings, increasing straight line braking and hang time.  Additionally, dynamic idle improves PID controller performance at zero throttle by permitting stronger braking of motors that are in positive airflow conditions.  

NOTE 1:  Dynamic Idle requires [fully functional BiDirectional DShot telemetry](https://github.com/betaflight/betaflight/wiki/Bidirectional-DSHOT-and-RPM-Filter).  

NOTE 2: **Dynamic Idle is off by default**.  To enable it, set `idle_min_rpm` to a suitable value in the CLI.  For typical 5" quadcopters 20 is a suitable starting point.  Please read the tuning section, below.

NOTE 3: Dynamic Idle must not be used with 3D mode.

NOTE 4: Transient throttle limit should be disabled while using Dynamic Idle (`set transient_throttle_limit = 0`)


### How does Dynamic Idle work?

#### Let's first explain how idle is managed without dynamic idle.  

Without dynamic idle, the lower limit of motor drive, under all conditions, is set by the `dshot_idle_value`. This defaults to 5.5%.   When throttle stick is at zero, the motors get 5.5% motor drive.  The PIDs cannot reduce motor drive below 5.5%, either. 

When we decelerate suddenly, airflow pushes up into the props from below the quad.  This slows them down a lot.  They may either turn so slowly that they can't start quickly when needed, or stop spinning completely, or even spin backwards.  This can lead to poor handling or total loss of control (motor desync), often most obvious at the end of fast flips or rolls.  To avoid these issues, we usually need to set the `dshot_idle_value` relatively high.  This limits hang time and doesn't allow strong motor braking.

If we want to quickly stop a fast roll move, we need to slow down the driving motors.  When we tell them to stop, they are moving quickly forward in the air.  The strongly positive airflow pushing into them keeps them spinning faster than they should be.  The PIDs would really like to send zero motor drive to them - maximal braking - but cannot send less than 5.5%, even though they are in no danger of stopping.  Because they can't be slowed down as much as they could, our ability to stop the turn is not as good as it could be.

Finally, lets consider an inverted drop, with a yaw while inverted.  While dropping cleanly, the motors will be at 5.5% motor drive, pushing downwards.  We might like to lower this value to improve 'hang time', but we can't, be cause it needs to be a high enough number to prevent desyncs.  If we try to yaw while inverted, the PIDs can only make the motors go faster, and speeding up the motors while inverted really limits hang time.  It would be good if we could slow them down a bit to help with the yaw, but we can't.

#### How does dynamic idle change things?

With dynamic idle enabled, the PIDs are allowed to send zero drive to the motors, so long as the motors do not spin too slowly.  The actual RPM is continually monitored using Bidirectional DShot telemetry.  Idle motor drive is dynamically adjusted to keep the slowest motor above the configured minimum RPM - even if its drive signal is zero.

That's how dynamic idle can greatly reduce the chance of a desync, and permits lower overall idle settings.

Because the motors won't slow down as much under strong reverse airflow conditions, they can start up quickly.  This provides improved zero throttle turn responsiveness, greater stability against cross-axis wobble when stopping a flip quickly, and improved zero throttle stability in flat drops.

Under strongly positive inflow conditions, where the motors will be spinning faster than expected, they can now be braked much harder, since the PIDs can send as little as zero drive to slow them down, instead of the mandatory 5.5%.  This is beneficial when quickly stopping a fast flip or roll, or when performing tricks during inverted drops.  

These are the three key elements to dynamic idle:

1.  We get the actual motor RPM via bidirectional DShot telemetry, and use this to prevent any motor falling below the set `idle_min_rpm` value.  

2.  We allow the PIDs to send motor drive to zero, provided that RPM is OK

3.  The `dshot_idle_value` becomes our minimum throttle value.

The dynamic idle minimum value becomes the effective minimum idle RPM, a kind of 'safety net' under which RPM should not fall.  

### Tuning.

`idle_min_rpm` is the key setting that enables Dynamic Idle.

The ideal setting for `idle_min_rpm` is a bit less less than the RPM as shown in the motors tab at idle.  If your idle was 5%, you'd set the motor to 1050 in motors tab, and check the RPM there.  If it was, say 2,200 rpm, then you might use 20 as your `idle_min_rpm`.

A value of 20 corresponds to 2,000 RPM and is a reasonable starting value for most quads.

Smaller quads rev faster, and a higher `idle_min_rpm` value, eg 25, may be more appropriate for a 2.5".
Similarly, larger quads can operate with lower 'idle_min_rpm' values, e.g. 14 may be more appropriate for 7" quads with larger motors.

If you already are running a known good DShot idle value, start off with that, with an `idle_min_rpm` value set as described above.

#### Can I use dynamic up to improve turn responsiveness even more?

Not really.  Most of the available benefits 'out of the box' once enabled.  Higher idle values will keep the motors spinning more quickly when we cut the throttle, and in drops, leading to more rapid spool up when needed, better zero throttle stability, and crisper flips - at the cost of less effective braking and a more floaty feel.   With the default DShot idle and an `idle_min_rpm` value configured as above, motor drive can fall to zero if the PIDs need it, so handling won't be a lot better if you increase idle further.  

#### I'm after lots of inverted hang time

Dynamic idle can allow longer inverted hang time.

To get it, aim for the lowest possible values of both DShot idle percentage and `idle_min_rpm`.  Typically you should set the `idle_min_rpm` set very close to or the same as your idling RPM from the motors tab.  Cut both values, stepwise, going as low as you can with both.

For example, if you were running DShot idle of 5% and that gave you 2000 rpm, you could try DShot idle of 4% and `idle_min_rpm` of 15.  If that was OK, then 3.5% and 13.  If hard flip stops get ragged, or you start tumbling out of the sky under reverse airflow due to a desync, that's too low.  You can typically get well below the default DShot idle value.

Low idle settings that maximise inverted time will not be so good for turn performance at zero throttle.


#### Is this useful for whoops?

Yes, for sure.  Whoops have low authority at idle that is often improved significantly with Thrust Linear.  This will result in active PIDs at zero and hover throttle, which can cause low RPM states. 

Whoops may require higher than default idle values to keep their motors spinning reliably.


#### I'm a very high turn rate Acro LOS pilot

When targeting very high maximum turn rates - 1800 deg/s for example - desyncs can be more of a problem.  For maximum acrobatic and hang performance, the `dshot_idle_value` can be lower than usual, and the `idle_min_rpm` value high enough to stop desyncs.  

Note that if the rpm from `idle_min_rpm` is higher than that of DShot idle, hovers may not be smooth, since the dynamic idle controller may hunt around a little.

#### I'm a race pilot

Racers typically use low throttle much less of the time, so you might wonder why you'd bother with something that only influences idle behaviour.  

There are two reasons.

First, braking.  Sure we want to go fast, but we also want to slow down.  If we can set idle lower than usual, we can brake harder.  Dynamic idle lets us do that.  We can go full throttle, and when we cut throttle, we will bleed off speed more quickly, making the next turn easier.  We will also be able to drop into dive gates more aggressively on cutting throttle.  Properly tuned dynamic idle can retain more precise attitude control while minimizing any passive thrust from idle in negative angle of attack maneuvers.

Second, we want strong turn performance at high speed.  Typically at high speed we have strong positive inflow. We can turn faster with dynamic idle active because under positive inflow conditions it can brake motors down to zero drive, helping turn in more quickly.  One race courses that involve high speed entries to maneuvers such as split-S or tight hairpins, these adjustments allow for more precise control and propwash management that allow for accurate rhythm-based turn entry and vertical maneuvers (e.g. repetitive dive/antigravity gates or inverted dive entries).

Tuning for racing would be a bit like optimising hang time, with low idle values.  The downside of low idle values is that when motors are spinning slowly, they can be slower to spin up, so if you do make a mistake and do a zero throttle reversal, it will be uglier, perhaps.  

For most racing setups, the higher inherent authority of the quad powertrain over the prop mass, higher tolerance for passive thrust at idle, and increased likelihood of entering negative angle of attack maneuvers from high speed all suggest that slightly higher dshot_idle_value and idle_min_rpm values are preferable (+0.5% and +200rpm respectively).


### The technical stuff

Dynamic idle adjustments are not instantaneous.  The correction factor uses a special PID controller where the adjustment is provided that is proportional to the difference between the needed and actual motor accelerations.  The delays and feedback mechanisms result in some potential for instability when extreme values are used.  

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
