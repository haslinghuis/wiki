<h3>Resource Remapping Command (available in v3.1)</h3>

<b>NOTE: this command is only newly available in v3.1 of Betaflight.</b>

The IO remapping allows you to configure the pins on the MCU to be utilised for various functions. This is the starting framework - more work can be done.

Pins are remapped using the resources command line interface command.

<b>resource [function name] [index] [pin]</b> e.g. <b><pre>resource MOTOR 1 A1</pre></b>

Where MOTOR is the function, 1 is the motor index (1 based e.g. 1-4 on a quad) and A1 is Port A pin 1 or more commonly referred to as PA1 in STM datasheet documentation. 

To remove a mapping, use <b>NONE</b> in place of PIN, e.g. 
<b><pre>resource MOTOR 5 NONE</pre></b>

Where a function does not require an index (i.e. there is only 1 possible pin assignment), e.g. BEEPER, SONAR_ECHO or SONAR_TRIGGER then the index <b>must</b> be omitted 
e.g. <b><pre>resource BEEPER B6</pre></b>

<b>resource</b> on its own will list all the available configurable options, and their current setting. This is the output to be added to the <b>dump</b> for use in backing up and restoring configuration. Note that this command will list all configured that would be allocated if used. 

As an example <b>resource</b> will show motors 1-8, but if your mixer is set to QuadX then only motors 1-4 will actually be used, if you change to Oct as the mixer (and reboot) then all 8 motors will be configured. 

<b>resource list</b> will list all pins and their current assignments, including all those in use by system components and <b>not</b> configurable by the user. It will also list the currently active DMA utilisation. Note for any adjustments made a save and reboot is required in order for those changes to be visible here. Consider this command the output of the currently active state.

<hr>
<center>
<img src="https://cloud.githubusercontent.com/assets/14850998/21921215/c5d3521c-d9a9-11e6-8ed8-c53afdbda50f.jpg" width="70%"><br>
Figure: How different resource command variation works
</center>  

#### A Joshua Bardwell Video:
[Resource Remapping- No more Custom Motor Mixer](https://www.youtube.com/watch?v=z5aO-3_n-Hs   )
