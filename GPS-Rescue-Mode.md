# **WHAT THIS IS:**

GPS Rescue Mode is intended to bring your quad back autonomously in case of an emergency such as loss of video or radio link. It is not meant to be a reliable Return to Home mode. Keep this in mind, and (other than for initial testing) only activate it to avoid losing your quad. In order to increase the probability of GPS Rescue's successful operation, please read this document and configure your system as best as possible for your particular environment and flying style.

# **REQUIREMENTS**

* GPS is required. The recommended models are Ublox m8n variants. This has been tested with 18x18mm m8n units, the Beitian BN 880 and other similar models. 
* Accelerometer must be enabled, Rescue Mode needs it to keep the quad leveled.
* Barometer is optional but recommended. We have tested with both on-board and external (i2c) units.
* **This mode does not use or need compass.**

# **DISCLAIMER**: 
*  This is an experimental feature.
*  Use with extreme caution.
*  This documentation WILL change so check this page often. 
*  If you plan on using this as a failsafe method you should ABSOLUTELY enable sanity checks!

## Go to the Betaflight Modes tab and add a switch for GPS Rescue Mode. Verify that the mode actually gets activated (of course no props).

Then configure the following parameters in the cli:

`set gps_rescue_initial_alt=[number] (default is 50)`

This is the most important parameter. When Rescue Mode is activated, your quad will point home and try to climb to a safe altitude relative to your takeoff point. This altitude will either be this parameter, or the maximum altitude recorded during flight +15m, whichever is highest. I personally like to make it 70 or 80 meters.

`set gps_rescue_ground_speed=[number] (default is 2000)`

This is the speed at which your quad will try to come back, in centimeters per second. I like 1500 (about 35 mph), but this setting depends on how and where you fly.

`set gps_rescue_angle=[number] (default is 32)`

This is the maximum allowed tilt angle for your quad when coming home. This setting may prevent it to reaching full speed, so you may have to experiment with it if you change the defaults. Note that the higher the angle, the harder it will be for the altitude controller to keep a stable altitude. When there is a chance of returning into head winds I like to set this parameter to 45 degrees.

`set gps_rescue_descent_dist =[number] (default is 200)`

This is the distance, in meters, at which your quad will start descending towards home.

### At this point you are ready to test Rescue Mode. 
 Wait for your gps to get a good fix. 
 By default your quad will not arm if you have less than `gps_rescue_min_sats` (default is 8) satellites. 
You can decrease this value in the CLI or even make it 0 if you just want to fly near yourself. Keep in mind that if you do not hav a GPS fix by the time you arm, your quad will not know where to return if you activate Rescue Mode, and it will simply land.
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

With this setting, GPS Rescue will activate in the event of a failsafe. However, it will return control to the user as soon as the radio link comes back. During this time the user should either activate Rescue Mode manually on the radio, just so that there cannot be an unexpected transition to manual control, or be ready to take over control at any moment. Our recommended approach is the first one, which requires having Rescue Mode on a switch if you want to use it for failsafe as well.

## **DEALING WITH FAILURES / SANITY CHECKS (VERY IMPORTANT): **

If you're using rescue mode in a supervised fashion (as a switch only with video), or in a place with no danger to surroundings (over water, etc), sanity checks are entirely optional.  If you plan on setting this as a failsafe (and most of the time even if you do not) you should highly consider enabling these.

`set gps_rescue_sanity_checks = RESCUE_SANITY_ON`

You can also set this to `RESCUE_SANITY_FS_ONLY` if you want it to only matter in a failsafe (unsupervised) condition.  

Sanity checks will ensure that you have a valid GPS fix, that the quad is spending most of its time approaching home, and also it will do its best to disarm if it detects a premature landing (not within the target home landing zone). 

## Common pitfalls
- Ensure that you are flying further than the minimum distance to home (100m by default) before testing GPS Rescue.
- For Betaflight versions prior to 4.0, it's highly encouraged to enable Air Mode, and optionally to finetune failsafe Stage1 settings, as a workaround for the crash detection issue immediately after activating Rescue Mode. Basically, ensure your settings will avoid the quad to be free-falling when entering into Stage2.
- When changing failsafe parameters with Betaflight Configurator 10.4 or lower, the failsafe procedure will be silently reset. Ensure that you set the failsafe procedure manually on CLI after saving modifications on the failsafe tab.

## Version History
**Betaflight 4.0**
* Prevent crash detection immediately after entering GPS Rescue mode (https://github.com/betaflight/betaflight/pull/7034)
* Allow minimum distance to home to be configurable (https://github.com/betaflight/betaflight/pull/6404)

**Betaflight 3.5.3**
* Fixed problem with MOTOR_STOP and auto-disarm activating when GPS Rescue is active (https://github.com/betaflight/betaflight/pull/6979);

**Betaflight 3.5**
* GPS Rescue activated on failsafe will check the quad to be at least 100m away from home, regardless of the Sanity check setting. If it's closer, it will drop. Non-failsafe activated GPS Rescue works as in 3.4.

**Betaflight 3.4**
* The Sanity check includes a test for the quad to be farther away than 100m from target home for the Rescue mode to be activated. When Sanity check is enabled, if GPS Rescue is activated (either manually or by failsafe) when the quad is closer than 100m from home, it will drop.