(To be edited)

What is a (Radio) failsafe?

-Failsafe is a safety feature, it can be triggered by a switch, the loss of radio link or an unexpected RCcommand pulse. The flight controller will behave as set in the failsafe tab of the configurator. ( Betaflight's default = Drop)
___

Why should I set my Failsafe?

-Failsafe place the flight controller in a "safer" state. Tipically, small Quad can fall without severe damage but with bigger quad, it might be better with landing. The basic rule behind it, it is better dropping from the sky unarmed than having a flyaway, randomly chasing people.

You might want to set the RCcommand on failsafe for each channel. I usually set throttle to hold leaving pitch, roll and yaw to "set". Auxilliary 1 switch to "unarmed", for me it is "set 1000" and AUX 2 "set 2000" ( I have a beeper).

I leave throttle to hold for 0.4 second, I can live with that. You might feel anxious about it and use auto, wich will put the signal to the rx failsafe set value. This is why it is so important on binding process to have the correct stick position.  
___
Which occasion it occurs?

-Broken, damaged or not the right lenght antenna on RX and or TX
-shadowing, multipath, antenna conductor touching carbon fiber or signal blocked by carbon fiber
-Too close of the VTX
-With satelite, 3.3v regulator problem
-


___
Basic Setups and Knowledge + Different Failsafe behavior
___
Quick and dirty troubleshoot:
___


Notes:
http://www.warms.com.au/news/2016/1/20/radio-failsafe

https://oscarliang.com/setup-failsafe/

http://ardupilot.org/copter/docs/radio-failsafe.html

https://github.com/betaflight/betaflight/wiki/Arming-Sequence-&-Safety