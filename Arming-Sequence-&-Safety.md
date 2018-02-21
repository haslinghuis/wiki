# Switch arming

The AUX channel configured for arming will choose a default value that falls outside of the configured arm range (as configured in the "Modes" configurator tab), this value is either 1 "step" (25us) above or below the configured range. This is a safety feature to ensure the default value would not cause accidental arming.

Note that this default value is overwritten by the first values data from the receiver, therefore bad RX initial states or failsafe settings can still cause issues.

# Arming Prevention

Betaflight 3.2 (and above) includes more detailed troubleshooting information for the case when a craft will not arm.

This information is available via:
- the CLI
- Betaflight OSD
- beep patterns
- Betaflight Configurator

Several of these conditions are to assist in preventing accidental arming via bad radio system configuration, unreliable/poor quality receivers and user mistakes.

These reasons for not arming are encoded as a set of flags (see [runtime_config.h](https://github.com/betaflight/betaflight/blob/master/src/main/fc/runtime_config.h)).

## CLI

Flags can be viewed via the `status` command.

Some targets with limited flash space will only provide the hexadecimal representation of these flags, in which case the active flags must be derived from the `armingDisableFlags_e` enum in `runtime_config.h` (ensure the version of the file you are looking at matches that of your firmware).

## Beeper

When arming is attempted and fails, if a beeper is connected to the flight controller it will emit a warning signal indicating the most important (lowest number) reason why disarming is disabled.

The signal is as follows:
- five short 'attention' beeps;
- a number of long beeps (may be 0);
- a number of short beeps with long intervals (may be 0).

The arming prevention condition that is active can be calculated as `(5 * <number of long beeps>) + <number of short beeps>`.

## Description of arming prevention flags

What each flag means and what you should do to (probably) fix the issue.

This list *should* be kept up to date with the code in `master` (`armingDisableFlags_e` in [src/main/fc/runtime_config.h](https://github.com/betaflight/betaflight/blob/master/src/main/fc/runtime_config.h#L37-L55)) so can be used to find what flag corresponds to a certain index, however if you run an older version you'd have to check this manually as mentioned above.


|Name|Description|Beeps in 3.2|Beeps in 3.3|Required Actions|
|---|---|---|---|---|
|`NOGYRO`|A gyro was not detected|1|1|You may have a hardware failure, if a previous firmware version works then it may be a firmware issue.|
|`FAILSAFE`|Failsafe is active|2|2|Rectify the failure condition and try again.|
|`RX LOSS` (1)|No valid receiver signal is detected|3|3|Your receiver is either faulty or has no link to the transmitter.|
|`BAD RX` (1)|Your receiver has just recovered from receiver failsafe but the arm switch is on|4|4|Switch the arm switch off.|
|`BOXFAILSAFE`|The 'FAILSAFE' switch was activated|5|5|See `FAILSAFE`|
|`RUNAWAY`|Runway Takeoff Prevention has been triggered| |6|Disarm to clear this condition.|
|`THROTTLE`|Throttle channel is too high|6|7|Lower throttle below `min_check`.|
|`ANGLE`|Craft is not level (enough)|7|8|Level craft to within `small_angle` degrees (default 25).|
|`BOOT GRACE`|Arming too soon after power on|8|9|Wait until `pwr_on_arm_grace` seconds (default 5) have elapsed.|
|`NO PREARM`|Prearm switch is not activated or prearm has not been toggled after disarm|9|10|Toggle the prearm switch.|
|`LOAD`|System load is too high for safe flight|10|11|Revisit configuration and disable features.|
|`CALIB`|Sensor calibration is still ongoing|11|12|Wait for sensor calibration to complete.|
|`CLI`|CLI is active|12|13|Exit the CLI.|
|`CMS`|CMS (config menu) is Active - over OSD or other display|13|14|Exit the CMS (or OSD menu).|
|`OSD`|OSD menu is active|14|15|Exit OSD menu.|
|`BST`|A Black Sheep Telemetry device (TBS Core Pro for example) disarmed and is preventing arming|15|16|Refer to the manual for your hardware.|
|`MSP` |MSP connection is active, probably via Betaflight Configurator|16|17|Terminate the Betaflight Configurator connection (disconnect).|
|`ARM SWITCH`|Arm switch is in an unsafe position|17|18|Toggle the arm switch to arm.|

(1) This may appear on the Betaflight OSD during flight, take it as a sign that your radio system is either faulty or you are flying at the edge of your range. Treat it the same you would an "RSSI critically low" warning.