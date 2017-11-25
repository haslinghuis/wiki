# Resource Remapping Command

**NOTE: this command is only available in version 3.1 and newer of Betaflight.**

The IO remapping allows you to configure the pins on the MCU to be utilised for various functions. This is the starting framework - more work can be done.

Pins are remapped using the resources command line interface command.

`resource [function name] [index] [pin]` (e.g. `resource MOTOR 1 A1`)

Where MOTOR is the function, 1 is the motor index (1 based e.g. 1-4 on a quad) and A1 is Port A pin 1 or more commonly referred to as PA1 in STM datasheet documentation. 

To remove a mapping, use `NONE` in place of PIN, e.g. `resource MOTOR 5 NONE`

Where a function does not require an index (i.e. there is only 1 possible pin assignment), e.g. `BEEPER`, `SONAR_ECHO` or `SONAR_TRIGGER` then the index **must** be omitted (e.g. `resource BEEPER B6`)

`resource` on its own will list all the available configurable options, and their current setting. This is the output to be added to the `dump` for use in backing up and restoring configuration. Note that this command will list all configured that would be allocated if used. 

As an example `resource` will show motors 1-8, but if your mixer is set to QuadX then only motors 1-4 will actually be used, if you change to Oct as the mixer (and reboot) then all 8 motors will be configured. 

`resource list` will list all pins and their current assignments, including all those in use by system components and **not** configurable by the user. It will also list the currently active DMA utilisation. Note for any adjustments made a save and reboot is required in order for those changes to be visible here. Consider this command the output of the currently active state.

Note that the `save` command must be used after changing pin mapping via the CLI.

<center>
<img src="https://cloud.githubusercontent.com/assets/14850998/21921215/c5d3521c-d9a9-11e6-8ed8-c53afdbda50f.jpg" width="70%"><br>
Figure: How different resource command variation works
</center>

## Wiki pages with examples of using the Resource Commands:   
[Remapping Motor outputs](https://github.com/betaflight/betaflight/wiki/Remapping-Motors-with-Resource-Command-(3.1))  
[Using SERVO_TILT](https://github.com/betaflight/betaflight/wiki/SERVO_TILT-for-3.1)   
[Using CHANNEL_FORWARDING](https://github.com/betaflight/betaflight/wiki/CHANNEL_FORWARDING-for-3.1)  
[Setup on a Fixed Wing Aircraft](https://github.com/betaflight/betaflight/wiki/Setup-for-a-Fixed-Wing-Aircraft)  