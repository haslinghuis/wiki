### Changes in Default Filter settings
From version 2.7 onward, the default filtering strength settings are _reduced_ in order to provide the best possible flight characteristics. You may find that after upgrading, your aircraft becomes very twitchy or uncontrollable. This can be caused if you have a noisy quad because the vibrations are now making it past the filter into the PID loop.
##### Note: Defaults values can be different across minor and major firmware versions. Always do a CLI DUMP (or a get 'partial name') to see what the default values are before making changes.  

### From Boris:
In case your setup is too noisy you need to adjust the filters. Here are some of the recommendations. The more filtering you use the less noise will be let into the system, but that will reduce the overall responsitivity of the pid controller and provide less stability in prop wash scenarios for example. Using as low as possible Dterm can help too.

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

### From ctzsnooze's post:  
The ideal value for filters is as high as you can go without getting hot motors etc. I don't think anyone can tell you how high you can go.

But your particular concern was that your motors still felt a bit warm. So I'd map out what dterm cut does while leaving gyro cut where it is. The only way to find out is to do the testing. Once you know the best value for Dterm cut, ie the value where motors are coolest, keep dterm cut and gyro cut in the same proportion from then on. If, despite finding the coolest value for dterm cut, they are still warmer than you'd like, try say reducing both by 20%. If they are cool and you want crisper performance, try increasing both by 20%, and keep going up until they become clearly warmer than with a lower value.

If you really want to optimise things you need blackbox. Your motors might just be warm because you fly hard! :-) There may be no PID oscillation at all. If that's the case, and you don't have blackbox, you can keep going up on the filters, both in a fixed proportion, keeping dterm cut above gyro cut to the same percentage - until the motors get obviously hotter or noisier, then back off a step. Then if you could be bothered, repeat the dterm cut optimisation until you have the filters set as best they can be.

Note that if you change P or D upwards or change props you may have to modify the settings.

On my setups 70 for gyro and 90 for dterm keep motors cool even with the most beat up props, flexy frames and oldest motors. 

See [BB logging page](https://github.com/betaflight/betaflight/wiki/Black-Box-logging-and-usage) for measuring noise and filter.

Post by ctzsnooze  
All filters add delay. Doubling slope on an IIR LPF doubles delay since the same 1st order filter is simply applied twice. None currently are FIR. FIR were evaluated and not as good as simple IIR. Dterm is IIR, gyro cut was biquad (i think it still is). There is a recent post about the notch filter that linked to the GitHub page where the Notch was discussed before implementation. Diagrams there show delay for different filter combinations. Lots of thought has gone into current filter design. 

Another post by ctzsnooze on using the Filters available in 3.x
http://www.rcgroups.com/forums/showpost.php?p=35764414&postcount=38600

#### Additional discussion on Filter Usage  

Post by Boris   
I still recommend that you slowly remove default filtering as well if your setup allows you for best results. Nowadays most do soft mounting so removing of filters can easily improve performance. The defaults are optimized for hard mounted medium noisy environment for safety. The best tuning performance is achieved with as less Du.  

I think best filter removal steps would be.  
set d_lowpass_type = PT1 should always be done first and I think possible on every setup.  

If it is still fine then:  
Remove notch 1  
Than if still fine remove notch 2  

Now if your setup is still clean you could even remove dterm notch. 
That will give you the best results. Sharpest response, easiest tuning and literally no prop wash on good setups.  
Note that default betaflight filters are made so every beginner can put a quad in air without burning his gear.  

Post by ctzsnooze   
When Boris was first putting notch filters into betaflight they were intended as narrow, specific point filters, to block tight peaks of noise - and for this they are really, really good.  
However, when set wide, and particularly if set wide with a low cutoff, they do have a 'tail' that causes both delay and phase shift. Those negative effects can extend some way below the lower cutoff point, especially if the filter is 'wide', ie there is a big difference between centre point and low point.  
For example, setting a notch centre point to 200 and its low point at 100 will have significant effects below 100, whereas setting it to centre of 200 and low point of 160 won't be nearly as much of a problem.  
Notch filters should be used primarily to control tall, discrete 'peaks' of noise, and only made just wide enough to control the peak.   
