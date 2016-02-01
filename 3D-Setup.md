This page is under construction and will be used to outline how to set up 3D mode on a multirotor and should list out the important and easily missed steps in order to avoid incidents.

I have taken my information from this post by Hwurzburg: https://github.com/cleanflight/cleanflight/issues/1032
As well as this pull request containing the same information: https://github.com/cleanflight/cleanflight/pull/1034

Step one:
Acquire multirotor and fit esc's with reversing/bi-directional capability.

Step two:
Read the required information, understand what is happening and test/prove everything on the bench BEFORE INSTALLING PROPS. There is twice as much potential for a user setup error to occur in 3D mode and cause an incident.

Step three:
Practice practice practice and more importantly, have fun!


Required information:

When FEATURE 3D is enabled, either by the configuration GUI or CLI command, it allows operation with bi-directional escs to provide inverted as well as normal, upright operation by allowing bi-directional operation of the motors to produce positive or negative thrust.

Setup:
-Enable FEATURE 3d in the firmware: This can be done via the checkbox on the config page of the configurator or via the CLI.
-Setup ESC to BIDIRECTIONAL MODE:
Consult your esc manual for how to enable bi-directional mode on your ESC

BL Heli ESC: (norm/reverse/birectional slider in BLHeli GUI) and set max pwm in the ESC GUI to the maximum your transmitter outputs on the throttle channel (normally 2000us) and min pwm to the minimum (normally 1000us) and the midpoint to halfway between (normally 1500us). The ESC will not ouput to the motors if its input is at the midpoint +/- a small deadband.

Kiss 24A ESC:  To teach the transmitter path (throttle path) the ESC / controller must be connected to a receiver
or FC, set the throttle signal at full throttle (peak throttle).Connect the LiPo to the ESC / controller.
A beep indicates the confirmation that the programming mode is activated. Now reduce the throttle
signal to minimum (normally 1000μs,), and wait for the restart of the speed controller (audible
signal high-low-high). The throttle pas is now programmed and the ESC/controller is ready for use.
 Caution: The loads, that arise for the ESC in the 3D mode, are up to 3 times higher!
3D mode: After the throttle travel has been programmed as described, the 3D mode can be activated
as follows: Disconnect the power supply, put the transmitter signal at full throttle, connect power
again, wait for beep. Adjusting the throttle to the middle position (half throttle path) and wait for
restart of the ECS/ speed controller (signal: high-low-high). The 3D mode is now active. Important!
speed controller now s tarts only at the throttle center position. Deactivation: Teach new master
travel

-Setup the following parameters in the CLI:

3d_deadband_high:(typically 1600) This is the lowest value for positive throttle output from the flight controller to an ESC when armed, the highest value for positive throttle output from the flight controller to an ESC when armed is max_throttle. The range between 3d_deadband_high and max_throttle is the total positive throttle output. 

3d_deadband_low: (typically 1400) This is the lowest value for negative throttle output from the flight controller to an ESC when armed, the highest value for negative throttle output from the flight controller to an ESC when armed is mincommand.

(Note: switch over point for positive or negative throttle output is midrc on throttle stick)

3d_neutral: (typically 1500) This is similar to how mincommand works in normal mode, but for 3d operation. 3d_neutral is the output value from the flight controller to the ESC when disarmed. Unless you have a specific reason not to, failsafe_throttle should be set to the same value as 3d_neutral.

3d_deadband_throttle: (typically 50) This is the throttle stick range around midrc which allows arming to occur, within this deadband the flight controller will output either 3d_deadband_high or 3d_deadband_low to the ESC. The value output (3d_deadband_high or 3d_deadband_low) is dependent on whether the throttle stick entered the deadband from a higher or lower value.

max_throttle: (typically 2000) This is the maximum value the flight controller will output to the ESC.

min_command: (typically 1000) This is the minimum value the flight controller will output to the ESC.

TODO

-Set a TX switch to control arming and set this up in the Mode tab of the configuration GUI

motor_stop and stick arming do NOT work in 3D mode for obvious safety issues. Arming will only occur when the throttle stick is centered (+/- 3d_deadband_throttle) and the ARMing switch is active. The motors will immediately spin at 3d_deadband_low or _high, depending on throttle position above or below midrc. Full power reverse will be at low stick and full power normal will be 
at full high stick. De-activating the ARMing switch will stop motors immediately independent of throttle stick position.

You will need to set the proper rotation for each motor by wiring order for the motor.
3D props are highly recommended to get reasonable and equal power inverted or upright.

Note: ONESHOT option works in this mode, transparently if the ESC supports it."