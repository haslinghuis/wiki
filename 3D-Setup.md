This page is under construction and will be used to outline how to set up 3D mode on a multirotor and should list out the important and easily missed steps in order to avoid incidents.

I have taken my information from this post by Hwurzburg: https://github.com/cleanflight/cleanflight/issues/1032
As well as this pull request containing the same information: https://github.com/cleanflight/cleanflight/pull/1034

**Step one:**
Acquire multirotor and fit ESCs with reversing/bi-directional capability.

**Step two:**
Read the required information and setup, understand what is happening and test/prove everything on the bench BEFORE INSTALLING PROPS. There is twice as much potential for a user setup error to occur in 3D mode and cause an incident.

**Step three:**
Practice practice practice and more importantly, have fun!


## **Required information:**

When FEATURE 3D is enabled, either by the configuration GUI or CLI command, it allows operation with bi-directional ESCs to provide inverted as well as normal, upright operation by allowing bi-directional operation of the motors to produce positive or negative thrust. You may need to set the proper rotation for each motor by wiring order for the motor, check the ESC manual to confirm.

Full power negative will be at low stick and full power positive will be at full high stick. Center stick will be zero throttle.

Switch arming is a must for 3D flight and can be set up in the Mode tab of the configuration GUI, motor_stop and stick arming do NOT work in 3D mode for obvious reasons.

Arming will only occur when the throttle stick is centered (+/- 3d_deadband_throttle) and the arming switch is active. The motors will immediately spin at 3d_deadband_low or _high, depending on throttle position above or below mid_rc.

De-activating the arming switch will stop the motors immediately independent of throttle stick position providing disarm_kill_switch is enabled. disarm_kill_switch is enabled by default. (This needs to be tested to confirm it causes disarm when entering mid_rc +- 3d_deadband_throttle.

3D props are highly recommended to get reasonable and equal power inverted or upright.

ONESHOT option works in this mode, transparently if the ESC supports it.

## **Setup:**

**Enable FEATURE 3D in the firmware:**

This can be done via the checkbox on the config page of the configurator or via the CLI.

**Setup ESC to BIDIRECTIONAL MODE:**

Consult your ESC manual for how to enable bi-directional mode on your ESC.

**Set both max_throttle and min_command before calibrating the ESC.**

* **BL Heli ESC:**

    (norm/reverse/birectional slider in BLHeli GUI) and set max pwm in the ESC GUI to the maximum your transmitter outputs on the throttle channel (normally 2000us) and min pwm to the minimum (normally 1000us) and the midpoint to halfway between (normally 1500us). The ESC will not ouput to the motors if its input is at the midpoint +/- a small deadband.

* **Kiss 24A ESC:**

    To teach the transmitter path (throttle path) the ESC / controller must be connected to a receiver
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
travel.

* **Simon K ESC:**
    To program a SimonK ESC to support 3D mode, one must edit the appropiate firmware configuration file to enable the following feature :
RC_PULS_REVERSE	= 1
    It is also recommended to disable stick calibration.
RC_CALIBRATION	= 0
    By default, SimonK sets the neutral throttle point (MID_RC_PULS) to halfway between minimum (STOP_RC_PULS, set to 1060), and maximum, (FULL_RC_PULSE, set to 1860) - so a value of 1460.  You can change these values as you desire, but these defaults work well.
MID_RC_PULS = = (STOP_RC_PULS + FULL_RC_PULS) / 2

**Setup the following parameters in the CLI:**

**3d_deadband_high:** This is the lowest value for positive throttle output from the flight controller to the ESC when armed, the highest value for positive throttle output from the flight controller to the ESC when armed is max_throttle. The range between 3d_deadband_high and max_throttle is the total positive throttle output range. 

**3d_deadband_low:** This is the lowest value for negative throttle output from the flight controller to the ESC when armed, the highest value for negative throttle output from the flight controller to the ESC when armed is min_command. The range between 3d_deadband_low and min_command is the total negative throttle output range.

**NOTE:** To find values for 3d_deadband_high and 3d_deadband_low the motors tab in the configurator can be used to find the value closest to mid_rc which spins all motors consistently in each direction. These numbers should be fairly evenly spaced from mid_rc and should be set as close to mid_rc as possible with mid_rc being centered between the values for 3d_deadband_high and 3d_deadband_low in order to rotate the motors at identical speed in either direction while the throttle is centered.

**3d_neutral:** This is the output value from the flight controller to the ESC when disarmed. This is similar to how min_command works in normal mode, but for 3d operation. Unless you have a specific reason not to, failsafe_throttle should also be set to the same value as 3d_neutral.

**failsafe_throttle:** This is the output value from the flight controller to the ESC once the failsafe conditions have been met. This should be set to the same value as 3d_neutral to cause the ESC to stop the motors under failsafe conditions.

**3d_deadband_throttle:** This is the throttle stick range around mid_rc which allows arming to occur, within this deadband the flight controller will output either 3d_deadband_high or 3d_deadband_low to the ESC. The value output (3d_deadband_high or 3d_deadband_low) is dependent on whether the throttle stick entered the deadband from a higher or lower value than mid_rc.

**max_throttle:** This is the maximum value the flight controller will output to the ESC. ESC calibration should be performed after changing max_throttle.

**min_command:** This is the minimum value the flight controller will output to the ESC. ESC calibration should be performed after changing min_command.