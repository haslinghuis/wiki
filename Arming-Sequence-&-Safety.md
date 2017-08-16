Betaflight 3.2 (and above) includes more detailed troubleshooting information for the case when a craft will not arm.

This information is available via:
- the CLI
- Betaflight OSD
- beep patterns
- Betaflight Configurator

These reasons for not arming are encoded as a set of flags (see [runtime_config.h](https://github.com/betaflight/betaflight/blob/master/src/main/fc/runtime_config.h)).

Some targets with limited flash space will only provide the hexadecimal representation of these flags, in which case the active flags must be derived from the `armingDisableFlags_e` enum in `runtime_config.h` (ensure the version of the file you are looking at matches that of your firmware).

Several of these conditions are to assist in preventing accidental arming via bad radio system configuration, unreliable/poor quality receivers and user mistakes.

## Description of arming prevention flags

What each flag means and what you should do to (probably) fix the issue.

- `NOGYRO`  
  A gyro was not detected.  
  You may have a hardware failure, if a previous firmware version works then it may be a firmware issue.

- `FAILSAFE`  
  Failsafe is active.  
  Rectify the failure condition and try again.

- `RX LOSS` (1)  
  No valid receiver signal is detected.  
  Your receiver is either faulty or has no link to the transmitter.

- `BAD RX` (1)  
  Your receiver has just recovered from receiver failsafe but the arm switch is on.   
  Switch the arm switch off.

- `BOXFAILSAFE`  
  See `FAILSAFE`

- `THROTTLE`  
  Throttle channel is too high.  
  Lower throttle below `min_check`

- `ANGLE`  
  Craft is not level (enough).  
  Levl craft to within `min_angle`

- `BOOT GRACE`  
  Arming too soon after power on.  
  Wait until boot grace time has elapsed.

- `NO PREARM`  
  Prearm switch is not activated or prearm has not been toggled after disarm.  
  Toggle the prearm switch.

- `ARM SWITCH`  
  Arm switch is in an unsafe position.  
  Toggle the arm switch to arm.

- `LOAD`  
  System load is too high for safe flight.  
  Revisit configuration and disable features.

- `CALIB`  
  Sensor calibration is still ongoing.  
  Wait for sensor calibration to complete.

- `CLI`  
  CLI is active.  
  Exit the CLI.

- `CMS`  
  CMS (OSD menu) is active.  
  Exit the CMS.

- `OSD`  
  No idea.  
  Turn it off and on again.

- `BST`  
  No idea.  
  Turn it off and on again.

(1) This may appear on the Betaflight OSD during flight, take it as a sign that your radio system is either faulty or you are flying at the edge of your range. Treat it the same you would an "RSSI critically low" warning.