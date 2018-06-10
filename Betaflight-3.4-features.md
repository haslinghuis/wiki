## Notes about some new 3.4 features and defaults

### I just want to fly, not read all this stuff...

No problem, flash the code, and do a brief test fly with the stock defaults.  **Both 8k and 32k gyro modes should fly on defaults**, though 32k may require more filtering - check motor temps after the first flight

If motor temps are OK, and handling generally alright, I'd recommend pasting into the CLI:

```
set iterm_relax = RPY
```

This will enable the term_relax code, which markedly reduces I bounce-back after flips or rolls, and allows higher levels of I than before.  Typically I can be increased by 50% or more to improve directional stability while flying in turbulent air or when approaching gates at high speed.

```
set rc_smoothing_type = FILTER
```

This activates low-pass RC input smoothing, which removes spikes and sharp edges that otherwise arise during rapid stick inputs.  It should keep the motors sounding smooth and running cool during frequent rapid stick inputs, without delaying responsiveness as much as the older interpolation method (which is still available as an alternative).  

Note that with any RC smoothing, the normal spikes from D weight or throttle inputs will be trimmed smooth.  While this improves efficiency, motor temperature, and smoothness of motor control signals, losing those spikes reduces the effectiveness of D weight, P, and throttle, during very rapid stick inputs.  After enabling RC smoothing, if you notice a small reduction in response to rapid stick inputs, consider increasing D weight and P by up to about 20%.

## The defaults are OK but I want to make some changes.  What do I need to know?

**The new default D weight value is 0.6**.  This is approximately equal to 0.8 in pre-3.4 versions.  More D weight means greater immediacy of stick responses, particularly to quick stick movements.  If the default of 0.6 doesn't feel responsive enough at the same rates as before, try a higher value.  1.0 is sufficient to overcome the normal damping behaviour that D itself would otherwise slow down responses to your stick inputs - the quad shifts from measurement to error mode of D calculation.  Values above 1.0 provide an additional 'feed forward' effect.  Too much D weight can feel excessively twitchy. 

**The new default D Setpoint Transition value is zero**, so if you previously used 1.0 or 0.5, and it now feels too twitchy around centre sticks, try your old setting.


If you have enabled rc_smoothing, you may find that you need to 


If your PID settings were higher than the current defaults, try with values for P, D, and D weight more like what you had.  

If the quad is a clean setup, typically run with good props 

### Dual lowpass filters on Gyro and D

3.4 introduces

### iTerm Relax

### Low-pass filter based RC smoothing