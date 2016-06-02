### Changes in Default Filter settings
From version 2.7 onward, the default filtering strength settings are _reduced_ in order to provide the best possible flight characteristics. You may find that after upgrading, your aircraft becomes very twitchy or uncontrollable. This can be caused if you have a noisy quad because the vibrations are now making it past the filter into the PID loop.

### From Boris:
In case your setup is too noisy you need to adjust the filters. Here are some of the recommendations. The more filtering you use the less noise will be let into the system, but that will reduce the overall responsitivity of the pid controller and provide less stability in prop wash scenarios for example.

* Default / Optimal flight performance:
 * gyro_lowpass = 100
 * dterm_lowpass = 110
 * gyro_lpf = OFF
* Slightly noisy setup:
 * gyro_lowpass = 80
 * dterm_lowpass = 100
 * gyro_lpf = OFF
* Very noisy setup
 * gyro_lowpass = 50
 * dterm_lowpass = 100
 * gyro_lpf = 188HZ
* 2.6.1 defaults:
 * gyro_lowpass = 80
 * dterm_lowpass = 70
 * gyro_lpf = OFF

### Example
Below is a snapshot of the Gyros (top) and PID sum (bottom) using default betaflight 2.8 PID settings. The Blackbox log viewer is zoomed out to 10% which helps to visualize the noise (thick gyro lines = noisy). Before changing the filter settings the quad was nearly unflyable.

Blackbox Gyro reading with Default 2.8 settings
![No filter](http://i.imgur.com/mMkDETV.png)

Blackbox Gyro reading with _Very Noisy_ settings from above
![Filtered](http://i.imgur.com/oOlGGtv.png)