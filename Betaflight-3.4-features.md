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

Note that with any RC smoothing, the normal spikes from D weight or throttle inputs will be trimmed smooth.  While this improves efficiency, motor temperature, and smoothness of motor control signals, losing those spikes reduces the effectiveness of D weight, P, and throttle, during very rapid stick inputs.  **After enabling RC smoothing, if you notice a small reduction in response to rapid stick inputs, consider increasing D weight and P by up to about 20%**. 

### The defaults are OK but I want to make some changes.  What do I need to know?

**The new default D weight value is 0.6**.  This is approximately equal to 0.8 in pre-3.4 versions.  More D weight means greater immediacy of stick responses, particularly to quick stick movements.  If the default of 0.6 doesn't feel responsive enough at the same rates as before, try a higher value.  1.0 is sufficient to overcome the normal damping behaviour that D itself would otherwise slow down responses to your stick inputs - the quad shifts from measurement to error mode of D calculation.  Values above 1.0 provide an additional 'feed forward' effect.  Too much D weight can feel excessively twitchy. 

**The new default D Setpoint Transition value is zero**.  If you previously flew with 1.0 or 0.5, to get a smooth centre feel, and it now feels too twitchy around centre sticks, try your old setting.  The default of 0 provides equal stick responsiveness regardless of stick position, and is recommended for racing.  Values under 0.1 are not recommended.

If your PID settings were higher than the current defaults, and the quad feels like it is a bit less responsive than before, try with values for P, D, and D weight more like what you had, and also try 20% above that. 

### What about these new dual filters?

3.4 provides dual low-pass filter capability for both gyro and D filtering.   The default is to have all four low-pass filters active at the same time, the dynamic notch filter on, but no other notch filters active.  This provides less filtering delay, typically, than before, but with better filter performance.  

The two gyro filters clean up noise before the gyro signal enters the PID loop.  P, I and D are then derived from that filtered data.  The two D filters are applied only to the D signal.

To determine the relative contribution of P and D noise to overall motor noise, analyse a blackbox spectrum from PID_P and compare that to a spectrum from PID_D.  Typically there will be more D noise than P noise.  Hence we usually need filter D more heavily than P. 

The more filtering we apply, the less noise gets through to the motors, and this keeps them sounding smooth and running cool.  If we apply too much filtering, the PID calculations will be delayed, and flight performance will suffer.  Without enough filtering, the motors may run hot, especially if the props get bent or the motor bearings are worn, etc.

Dual PT1 filters allow fine tuning of the amount of filtering to match the noisiness of the quad.

Here are some examples of typical filtering setups, that can be pasted into the CLI:

A really good quad with a very solid frame, new motors, and only ever flown with clean well balanced light props:

```
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 150
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 400
set dterm_lowpass_hz = 100
set dterm_lowpass2_hz = 250
```

A typical quad on a decent frame that needs to tolerate bent props:

```
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 120
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 300
set dterm_lowpass_hz = 80
set dterm_lowpass2_hz = 160
```

A beat up quad on a sloppy frame that often is flown with bent props:

```
set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 90
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 180
set dterm_lowpass_hz = 70
set dterm_lowpass2_hz = 140
```

The higher the filters and the better the state of the quad, the better it will fly.

## Should I keep the dynamic notch filter on always

Probably.  It does add delay, but really helps if a prop gets bent.  For super clean setups where performance is everything, try with it off.  Make a very heavily D filtered profile to limp home if needed, otherwise motors can overheat a lot.

## Do I need the fixed notch filters anymore?

Short answer:  No.  They cause a lot of delay, and dual PT1 filters usually are enough.

Long answer: Fixed notch filters may be useful if a log spectrum shows a clear noise peak despite the dynamic notch.  Typically a problematic peak will appear at prop resonant frequency on flexible frames.  The only way to know for sure is to get a blackbox log and use PID-Analyzer or Blackbox Explorer to perform spectral analysis.  Prop resonant frequency can be determined using an audio spectrum analyser and 'plucking' the propeller, sometimes just setting a D notch at that frequency can be useful.  

