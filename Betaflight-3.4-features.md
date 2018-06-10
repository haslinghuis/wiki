## Notes about some new 3.4 features and defaults

### I just want to fly, not read all this stuff...

No problem, flash the code, and do a brief test fly with the stock defaults.  Both 8k and 32k gyro modes should fly on defaults, though 32k may require more filtering - check motor temps after the first flight

If motor temps are OK, and handling generally alright, I'd recommend pasting into the CLI:

```
set iterm_relax = RPY
set rc_smoothing_type = FILTER
```

This will enable the term_relax code, which markedly reduces I related bounce-back issues, and enable low-pass RC input smoothing, which should keep the motors sounding smooth and running cool during rapid stick inputs with minimal delay.  This code allows higher levels of I than before; you may find you can increase I by 50% or more

The new default D Setpoint Transition value is zero, so if you previously used 1.0 or 0.5, and it now feels too twitchy around centre sticks, try your old setting.

The new default D weight value is 0.6.  This is approximately equal to 0.8 in pre-3.4 versions.  D weight, assuming D Setpoint Transition of zero, determines centre stick responsiveness.  To choose a suitable value, set D Setpoint Transition to zero, then adjust to the desired degree of responsiveness around centre sticks.

If you have enabled rc_smoothing, you may find that you need to 


If your PID settings were higher than the current defaults, try with values for P, D, and D weight more like what you had.  

If the quad is a clean setup, typically run with good props 

### Dual lowpass filters on Gyro and D

3.4 introduces

### iTerm Relax

### Low-pass filter based RC smoothing