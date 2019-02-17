### Overview

Integrated Yaw is a feature which corrects a fundamental issue with quad control: while the pitch and roll axis are controlled by the thrust differentials the props generate yaw is different. Yaw gyro changes happen primarily due to inertia when the rpm of the motors change. A constant acceleration of yaw requires a constant rate of increase/decrease in the rpm of the yaw motor pairs.

This means that on Yaw I has the effect of P and P the effect of D. FF is a control element that adds impetus based on the acceleration in setpoint rate.

This results in several issues on yaw. Typically yaw will react very fast initially, then fall back and catch up again over time.

Integrated Yaw fixes this by integrating the output of the yaw pid before applying them to the mixer. This normalizes the way the pids work. You can now tune as any other axis. It requires use of absolute control since no I is needed with Integrated Yaw. 

### Setup

Here are the CLI commands to enable it:

```
set use_integrated_yaw=on
set iterm_rotation=off
set abs_control_gain=10
```
Now reconfigure your Yaw pids to something like P/I/D/FF = 30/0/20/60. Make sure I is zero since significant I leads to strong oscillations. Pitch and roll might require slight retuning - primarily reducing I a bit since absolute control acts similar to a second I term.