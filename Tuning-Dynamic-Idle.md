### Intro

Dynamic Idle allows the PID controller to use ``dshot_idle_value`` as an idle set point instead of a floor value as previously implemented. In the absence of PID correction ``dshot_idle_value`` serves as the set-point value that is input to the motors. From that set-point value the input can be raised or lowered by PID correction. Dynamic Idle allows  increased braking on the decelerating motors and reduced input for the accelerating motors. 

A new parameter, ``idle_min_rpm`` is introduced to provide an rpm based floor-value as a safety net against motor desync. By utilizing an rpm based value, the floor can be set lower while still avoiding desyncs as it will stay consistent irrespective of air flow, i.e. it will not be driven lower by reverse flow conditions.

This new approach is possible because the FC can now receive actual real time rpm information from the ESCs via bidirectional dshot and use this information to implement the ``idle_min_rpm`` floor. Dynamic Idle therefore requires bidirectional dshot. It is currently not supported when 3D mode is enabled.

### Configuring Dynamic Idle

Dynamic Idle has two key parameters: ``dshot_idle_value`` and ``idle_min_rpm``. Setting ``idle_min_rpm`` to any non-zero value enables Dynamic Idle. ``set_transient_throttle_limit`` should be turned off when using Dynamic Idle.

##### dshot_idle_value
When Dynamic Idle is enabled the function of ``dshot_idle_value`` is modified: it now specifies the throttle set point to be used when zero throttle input is provided. It will often be necessary to slightly increase ``dshot_idle_value`` to achieve the same idle rpm as a quad that is not using Dynamic Idle because the PIDs can raise or lower motor output from the dshot_idle_value. If you have used air-mode on a 3-position arm switch you will know how switching on air-mode increases idle rpm a bit. When Dynamic idle is used this increase no longer occurs so ``dshot_idle_value`` needs to be adjusted slightly higher. Units of ``dshot_idle_value`` are: percentage of maximum throttle X 100 (example 5.5% X 100 = 550)

##### idle_min_rpm
``idle_min_rpm`` specifies a floor value that motor rpm can fall to when PIDs temporarily driving the motor input below ``dshot_idle_value`` or due to reverse air-flow conditions. The value of ``idle_min_rpm`` should be set lower than ``dshot_idle_value`` under normal circumstances. Lower ``idle_min_rpm` values give the PIDs more latitude to slow down motors, but setting the value too low can cause poor responsiveness and motor desyncs. A good starting point for 5" quads is 20. Smaller quads can go higher. If you have responsiveness issues or motor desyncs increase the value. If your quad shows too much idle-thrust in reverse flow conditions which does not respond to decreasing ``dshot_idle_value`` then decrease ``idle_min_rpm``. Confusingly the units of ``idle_min_rpm`` are not rpm, they are: rpm / 100.

Verify that the number you use is significantly lower than your normal idle rpm. One way to do this is by setting ``debug_mode`` to ``rpm_filter`` and logging the quad's rpm without Dynamic Idle. Convert the debug log numbers (in hz) to rpm by multiplying by 60 and divide by 100 to arrive at a number in ``idle_min_rpm units``. Your idle_min_rpm should be significantly below this number.

Alternatively you can use the motor's tab in BF Configurator to estimate idle rpm. Increase the sliders to reflect your ``dshot_idle_value`` - so say ``1055`` for ``5.5%`` dshot_idle_value. The rpm will now be displayed above the slider.


### Tuning Dynamic Idle

The optimal exact balance between ``dshot_idle_value`` and ``idle_min_rpm`` will depend on craft, flying style and rates. Very high rates (say mine of 1800 deg/s) lead to dramatic currents at the end of a roll, so a higher ``idle_min_rpm`` may be necessary to avoid motor desyncs. A racer might prefer very good response and low rates - therefore a low ``idle_min_rpm`` and a high ``dshot_idle_value`` may be useful. Freestylers and LOS pilots will appreciate very low thrust at idle and might want to use less ``dshot_idle_value`` while using a bit more ``idle_min_rpm`` to avoid desyncs at high rates.

