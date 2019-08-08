### Intro

BF 4.1 contains a new feature for use with dshot rpm telemetry: dynamic idle. It's purpose is two-fold: allow better desync protection and make more braking torque available to the flight controller.

### Desync Protection

The most difficult environment for an ESC to manage is when a lot of current flows in the motor windings at low rpm. The reason is that the ESC needs to detect motor position by measuring the induced voltage on the non powered winding. That voltage is proportional to rpm hence at lower speed it's more difficult to detect. And secondly there is some cross talk across windings that's proportional to the current in the active windings. That current scales with pwm duty cycle and prop load.

Specifically these conditions are worst when air flows from the back to the front of a motor (reverse flow) and when max power is requested from the motor at the same time. This happens for example at the end of a roll, where full power is requested of the motors that have been slowed down due to the high rotational speed of the quad which creates reverse flow.

We traditionally tune our dshot_idle_value to satisfy two goals: 
- Low idle so we don't get much thrust at idle.
- Enough idle so responsiveness is good (it takes a long time for props to speed up from very low rpm).
- Enough idle so motors don't desync in reverse flow conditions.

``dshot_idle_value`` is specified in percent * 100, so 500 is an idle throttle of 5% and puts 5% throttle to the motors when your throttle stick is at zero.

Dynamic Idle introduces a second setting: ``idle_min_rpm`` which sets the minimum rpm that will be allowed for any motor. It's specified in ``rpm / 100``. So say you're flying ``2400kv`` motors on ``4s``, giving you ``~35520 rpm`` at max throttle. Say we want the min rpm to be similar to maybe 4% of max. That's 1420.8, so you would ``set idle_min_rpm = 14``. Some quads - esp. high rates LOS quads may need as much as ``28``. 

Effectively your motors will idle at the higher one of (a) the motor speed caused by idle throttle and (b) ``idle_min_rpm``. ``dshot_idle_value`` is a static value, but ``idle_min_rpm`` results in a dynamic addition if one of the motors gets too slow. As the ``idle_min_rpm`` limit approaches the FC increases output equally to all motors to ensure that the minimum rpm constraint is satisfied. 

This setting doesn't replace dshot_idle_value, but complements it. It should be set such that in normal conditions ``dshot_idle_value`` still determines how fast the motors spin. ``idle_min_rpm`` is merely the insurance policy that keeps motors spinning fast enough in reverse flow conditions.

Clearly this allows to prevent low rpm desyncs without raising the dshot_idle_value, so you may fly with less idle thrust. The exact balance between ``dshot_idle_value`` and ``idle_min_rpm`` will depend on craft, flying style and rates. Very high rates (say mine of 2000 deg/s) lead to dramatic currents at the end of a roll, so a higher idle_min_rpm may be necessary. A racer might prefer very good response and low rates - there a low ``idle_min_rpm`` and a high ``dshot_idle_value`` may be useful. Freestylers and LOS pilots will appreciate very low thrust at idle and might want to use less ``dshot_idle_value`` while using a bit more idle_min_rpm to avoid desyncs at high rates.

### Increased Braking Torque

Dynamic idle internally uses a ``dshot_idle_value`` of ``1``, but adds a constant idle throttle to compensate and deliver the same idle rpm. This has a big advantage: now motors can be decelerated by the FC sending the minimum dshot motor value. Previously the FC would never send less than the ``dshot_idle_value``.

In practical terms this means that noise or small pid action will no longer result in an rpm increase at idle - a welcome change resulting in more hang time. Also during quick maneuvers the added 6% or so of braking torque will help the quad perform with more precision and lower average rpm. This will lower overshoot and make light weight quads less jumpy in airmode situations.

### Suggested Tuning

Start out with ``idle_min_rpm = 14`` and ``dshot_idle_value = 1``. This means that your motors will sit at idle_min_rpm when idle. Fly around, do max rate rolls etc and check for desyncs, increasing ``idle_min_rpm`` as needed. Now increase dshot_idle_value to get the preferred trade-of between idle thrust and quad response.