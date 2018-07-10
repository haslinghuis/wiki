# **DISCLAIMER**: 
* this is an experimental feature.
*  Use with extreme caution.
*  This documentation WILL change so check this page often. 


## Go to the Betaflight Modes tab and add a switch for GPS Rescue Mode. Verify that the mode actually gets activated (of course no props).

Then configure the following parameters in the cli:

`set gps_rescue_initial_alt=[number] (default is 70)`


This is the most important parameter. When Rescue Mode is activated, Your quad will point home and try to climb to this altitude relative to your takeoff point. I personally like to make it 70 or 80 meters.

`set gps_rescue_ground_speed=[number] (default is 1500)`


This is the speed at which your what will try to come back, in centimeters per second. I like 1500 (about 35 mph), but this setting depends on how and where you fly.

`set gps_rescue_angle=[number] (default is 30)`


This is the maximum allowed tilt angle for your quad when coming home. This setting may prevent it to reaching full speed, so you may have to experiment with it if you change the defaults. Note that the higher the angle, the harder if will be for the altitude controller to keep a stable altitude. When there is a chance of returning into head winds I 
  like to set this parameter to 45 degrees.

`set gps_rescue_descent_dist =[number] (default is 200)`


This is the distance at which your quad will start descending towards home.

### At this point you are ready to test Rescue Mode. 
 Wait for your gps to get a good fix. 
 By default your quad will not arm if you have less than gps_rescue_min_sats. 
You can decrease this value in the CLI or even make it 0 if you just want to fly near yourself.
## We suggest the following procedure:

Fly in a straight line for at least 100 meters past your descent distance. For example, if your descent distance setting is 150 meters, fly to 250 meters. As you fly, the home arrow should adjust to point towards home.



 ## **VERY IMPORTANT: **  if your arrow does not point towards home, **DO NOT** activate GPS Rescue. Your quad will try to fly in the direction of the arrow if you do.
Activate GPS Rescue. 

## **IMPORTANT**: be ready to deactivate the mode and take back control if your quad does not point towards you and starts making its way home.

If everything goes well, your quad will come back towards you and start descending. Do not let it get too close to the ground or to yourself because the landing functionality is not included in current builds. Your quad may just crash near your or overshoot you.

You may have noticed that the quad had a hard time keeping a stable altitude. Sometimes this happens when the GPS altitude reading is unstable, so the controller is aiming for a moving target. If you had a very stable altitude reading and the quad still could not stabilize within ten meters of your desired target altitude, you may have to adjust the altitude throttle PID gains. These are the parameters:

`gps_rescue_throttle_P`
`gps_rescue_throttle_I `
`gps_rescue_throttle_D`


We do not expect that most people will have to fine tune the navigation speed gains, but just in case the PID gains are:

`gps_rescue_velocity_P = 80`
`gps_rescue_velocity_I = 10`
`gps_rescue_velocity_D = 20`


After your quad reliably returns home once, you may want to test it at progressively larger distances and directions. When you have a reasonable level of trust in the feature, you may want to set your failsafe to GPS_RESCUE:

`set failsafe_procedure = GPS-RESCUE`