This page is meant to provide a more in depth explanation of the changelog for each release so that users can understand the consequences of the changes made. Please remember to read the changelog for each release first.

# 2.4.0 (proposed)
- PIDs scaled to voltage
- oneshot42
- cleanup code to get more flash space
- add some new targets. Lux board will get own target for example.
- Remove old mixer. Air mode will be default with some kind of idle up method to still simulate current default mixer
- consider to have a configurator for betaflight to bring things more centralized like flashing and documentation and make configuration easier.
- Fix pitch/roll in 3D throttle deadband
- Add a serial buffer to speed up BB Log downloading

# 2.3.5
Airmode Saturation Behaviour (Same like 2.1.6 and 2.2.0. Some setups cannot deal with aggressive corrections. This version only gives solution for those who have issues with double rolls etc. Basically the mixer mechanism is changed like in 2.1.6 and 2.2.0. It might feel a bit softer than 2.3.4 in hard manouvres, but that has been proven to work on all setups in the past.

"...spazzing out is a result of not enough power to get desired correction. 
I restored the original mixer behaviour:...
When mixer comes to the conclusion that desired PIDsum cannot be achived with the current motor powers it has to do something else than just mixing. This is what I mean with saturation scenarios."

# 2.3.4

Softer D approach as default baseline. Goal is to eliminate more D noise which should allow for higher D values for noisier quads. Rc_smoothing disabled by default as the new gyro delta approach should smooth out D anyway. See 2.3.3 for important details.

# 2.3.3

A lot of improvements and changes in this release. 3D airmode is much improved. Use set gyro_lpf = off to allow for 2khz mode. On F1 targets remember to also disable mag/baro/acc. **IMPORTANT: Lower your D values (to 0-5) when first enabling 2khz mode and check motor temps frequently! Some users have reported cooked motors as a result of D values that are too high.**

#2.1.6

A stable release. Use this if you are just getting started with betaflight and do not want 2khz mode or 3D airmode.