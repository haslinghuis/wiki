<h3>Resource Remapping Command (available in v3.1)</h3>

<b>NOTE: this command is only newly available in v3.1 of Betaflight.</b>

The IO remapping allows you to configure the pins on the MCU to be utilised for various functions. This is the starting framework - more work can be done.

Pins are remapped using the resources command line interface command.

<b>resource [function name] [index] [pin]</b> e.g. <b><pre>resource MOTOR 1 A1</pre></b>

Where MOTOR is the function, 1 is the motor index (1 based e.g. 1-4 on a quad) and A1 is Port A pin 1 or more commonly referred to as PA1 in STM datasheet documentation. 

To remove a mapping, use <b>NONE</b> in place of PIN, e.g. 
<b><pre>resource MOTOR 1 NONE</pre></b>

Where a function does not require an index (i.e. there is only 1 possible pin assignment), e.g. BEEPER, SONAR_ECHO or SONAR_TRIGGER then the index <b>must</b> be omitted 
e.g. <b><pre>resource BEEPER B6</pre></b>

<b>resource</b> on its own will list all the available configurable options, and their current setting. This is the output to be added to the <b>dump</b> for use in backing up and restoring configuration.

<b>resource list</b> will list all pins and their current assignments, including all those in use by system components and <b>not</b> configurable by the user. It will also list the currently active DMA utilisation. Note for any adjustments made a save and reboot is required in order for those changes to be visible here. Consider this command the output of the currently active state.