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

### Filters were changed in Version 3.0 

#### What Defaults changed in 3.1.7?
Boris's answer:  
Basically people kept complaining that betaflight default D was too conservative and therefore changed.  
Setpoint transition has been disabled (1.0) to give more linearity over the entire stick.  
I still recommend that you slowly remove default filtering as well if your setup allows you for best results. Nowadays most do softmounting so removing of filters can easily improve performance. The defaults are optimized for hard mounted medium noisy environment for safety. The best tuning performance is achieved with as less Du.

I think best filter removal steps would be.  
set d_lowpass_type = PT1 should always be done first and I think possible on every setup.  
If it is still fine than:  
Remove notch 1  
Than if still fine remove notch 2   
Now if your setup is still clean you could even remove dterm notch.  
That will give you the best results. Sharpest response, easiest tuning and literally no prop wash on good setups.  

Note that default betaflight filters are made so every beginner can put a quad in air without burning his gear.   

Dterm lowpass is always needed. Never remove that! Even on the cleanest setup.

I think if you really put some effort to build a clean setup you easily can get away without most of other filter settings.
To test that I was able to repeat it on 3 of my builds.  Hardware softmounting is always filtering without extra penalty.  

The most difference with and with filtering comes from the steep gyro changes. The more filtering there is the slower P will react on high gyro changes and translates itself to more prop wash and less direct feel.  
As I said defaults are meant to be safe and protect against most common vibrations shown across many hardmounted 4 / 5inch setups. But more and more people care about clean setup nowadays than lets say 1 year ago and that means you may get much better running setup with some filter removals. 

#### What is the difference between PT1 vs BIQUAD filters   
Answer by pete_oz:  
A biquad filter is more aggressive (it can filter the noise better at the cost of extra delay). I have no technical expertise to explain how each filter works (perhaps others can explain it for us) but basically based on latest Boris's recommendation we should be using PT1 unless there is too much noise on your copter.

Answer by ctzsnooze:  
PT1 is a standard -6db/octave 'infinite impluse type' IIR type low pass filter. Signal amplitude is halved at the set frequency and then is reduced by a factor of four times each time frequency doubles above that. Delay is approximately 1ms at 100Hz (2ms at 50Hz etc). Phase shift is 45 degrees at cutoff frequency. This filter is the digital equivalent of a simple analog low pass RC filter.  

BiQuad is a steeper -12dB/octave low pass filter. Signal amplitude is halved at the set frequency and then is reduced by a factor of 8 times each time frequency doubles above that. The 'cost' is that delay and phase shift is doubled. Delay is approximately 2ms at 100Hz (4ms at 50Hz etc). Phase shift is 90 degrees at cutoff frequency (I think that's correct). Basically twice as steep a cut, but more delay.  

Answer by Boris:  
Ctznooze explained it already, but here a very easy explanation for those who don't understand how filters work.  
PT1 is a less good filter, but therefore much faster than BIQUAD. Dterm speed of reaction gets greatly improved with PT1 therefore and the pid controller gets a faster reaction in quick disturbance cases.  
The main performance difference comes into play during very rapid accelerations and decelerations of gyro changes.  
You could say that rising and falling edge scenarios from gyro changes are better followed with PT1 than with any other higher order filter.  
Though BIQUAD coefficients also can be modified to be same to PT1.  

#### What is the difference between the gyro notch and the D-term notch?  
Answer by Boris:  
Gyro notch is applies to gyro input and dterm notch on dterm output.  
But those are in line. Gyro noise at 300hz will also result to dterm noise at 300hz  
Loop:
Gyro->PID->motors
And again  

#### How should I set Dterm Notch Filter? Do I set it to the same value as Gyro Notch Filter e.g. 285Hz?
Answer by pete_oz:
It's difficult to answer this question because in BB spectro analysis we are unable to produce spectro graph that would show us state of noise after gyro notch filters were applied but before D notch filter is applied.  
We can see spectro graph before gyro notches are removed ("pre-notch") and can only compare it gyro spectrum graph after all filters (incl. D notch) are applied.  
In order to be able to identify this better the developers would have to implement debug mode that would allow us to see state of noise after gyro notch filters but before d notch filter is applied.
I remember someone was requesting this functionality on Github but the request was dismissed with explanation given (please don't quote me this) that D term noise pattern is identical to that of gyro noise except for the magnitude of the noise.

When asking same question in the past (how to set my D notch filtering) I believe I was given the answer to set my D notch filtering for example between my 2 gyro notches (if I have a gap there) or otherwise set it to where I have most noise.  

Answer by ctzsnooze:
Well, first get the noise level under control with basic filtering (no notch filters at all). By that I mean, configure the basic gyro filter appropriately, either BiQuad or PT1, until your P trace on blackbox is 'smooth enough'.  

If the P trace has a prominent noise peak, apply a gyro notch filter to specifically block that out - but only if the amplitude is big enough that you can see it in the P trace itself. If the noise is less than 1% of signal, like if you can't really see it in P trace compared to signal, ignore the spectrum and don't use any notch.

If you need two gyro notch filters for speciific gyro peaks, use them.  

After doing all this, then look at the Dterm trace. You must have some basic Dterm filtering. If the Dterm trace appears to have less than a few percent noise, you don't need to do anything, you could conceivably use less filtering an get better performance. If you have a broad band of noise on D, then you may need to lower the overall D lowpass or use a BiQuad on D.

Only then would you think about adding a D notch filter, and only to deal with a very specific peak that had got through the gyro notch filters.

Finally check the motor traces at different stages in a typical flight. Provided that oscillations are less than a couple of percent, they can basically be ignored. We are not trying to get a flat spectrum, just to make it so that signal to motors is 'clean enough'. If we filter super hard, we may get rid of high frequency noise, but may end up exaggerating low frequency wobbles. There is some 'sweet spot', and it differs according to what you want to achieve.

Full optimization simply can't be done without a blackbox, quite a lot of time, and knowing what you're doing. Think of it like you are reprogramming the ECU of your car.

Also be aware that if you change your props, if they get some nicks or bends, or if you get a little bearing wear, or even if you just change ESCs, all your careful optimizations done with clean new original gear won't work so well.

Reducing filtering to the bare minimum, keeping motors perfect, and only flying clean props = fantastic performance and hardly any propwash wobble (assuming light props, low rotational mass, and free-revving powerful motors).

For me, I'd rather fly. I set my filters relatively quite low, and don't care much about a little propwash. Motors stay cool, tolerate bent props really well, don't get burning hot at the drop of the hat. Turn P down on more powerful setups. But yeah, I get propwash on hard 180 stops, so I just live with it and learn to fly smoother arcs :-)    

No one should use motor signal as the input source in blackbox when modifying filters.

Pterm noise is the same as gyro noise multiplied by your P factor. In BlackBox, gyro or P traces should be used as the input source for tuning gyro filters.

Dterm noise can only be evaluated using the Dterm trace in BlackBox logs. Be aware that Dterm is calculated *after* all the gyro filtering, so always first optimise the gyro filters, then worry about Dterm filtering independently. A Dterm notch should only be used when there remains a peak in Dterm *and* where that peak is of such magnitude to be a significant contributor in terms of your final result. Otherwise don't use it at all. Just randomly setting the Dterm notch in-between the gyro notches is not likely to work well.

By comparing noise levels and spectrum between P and D in blackbox, it's actually quite easy to visualise what the Dterm filtering is doing. And of course you can turn it off and do a short test run :-) just not for long or you may overheat.

The only role that the motor traces have is to look at your overall 'end result' - to visually quantify the amount of energy in oscillations of any kind, with the goal of keeping the total oscillation energy low enough to be not a practical issue. They should not be used as a blackbox input when tuning filters.   

Comment by zenkinsw:
I'm not too technical but once I switched D lowpass to Pt1 it does handle windy days much better, P hunts much less, and less prop wash too, basically less bumpy fly, but trade off with some more D noises motor get hotter.
I think people want to try PT1 need to lower down D back to around 24 first, tune it like the old days, actually I found much better as I know when to stop adding numbers lol, also with dshot much easy to see over Dgains symptoms so you know when to back down.
For racing I think Biquad still best if don't mind a touch of prop wash but benefits will more efficient and faster motors.  

#### How do I see the dterm trace? Roll/pitch D graph in BB log??  
Answer by ctzsnooze:  
Yep, click on the icon at the right near the parameter to select that one.  
For Dterm just graph it, select either roll or pitch (they can be different).  
Remember that you can scroll to a 'start' point in the log, press 'i' on the keyboard, then scroll a bit further on, press 'o' on the keyboard... now your graph only shows that region of the log.  
Very, very useful. Allows you to look at hover throttle, mid throttle, high throttle independently, often the peaks are different at different RPM. Looking at noise throughout the entire trace is OK but it can be more productive to focus on the throttle range you care about most (or the range with the most noise).  
Be sure to edit your BBLog settings to disable all expo and set all gains to 100% in the first instance. Expo on BBLog traces is very confusing, much better disabled.  (S & X keys)

Finally, use the spectrum only to determine the *relative* distribution of the frequencies, not the absolute magnitude of the noise. In other words, do not worry about the absolute 'height' of the spectrum. It is always bigger if you include more data. For the same included amount of time, a bigger spectrum means more noise, but you must select the same length range to do valid comparisons.  
To evaluate the overall magnitude of noise, look at the Motors traces - the wiggly lines themselves - with no expo and 100% scale. Eyeball how much noise you reckon there is, as a proportion of the total motor signal itself. 

Post by ctzsnooze on using the Filters available in 3.x
http://www.rcgroups.com/forums/showpost.php?p=35764414&postcount=38600

### Additional discussion on Filter Usage  

#### Post by Boris   
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

#### post by r.a.v.
As explained earlier PT1 is a first order filter and biquad is a second order filter.  

A main difference is also how steep the cutoff of the filter is. 
Here's a graph that shows the difference between the two. (X is frequency in Hz, Y is magnitude in dB).  
You can see that the biquad does not influence frequencies below the cutoff as much as the pt1 filter, but much stronger above.  
Keep in mind that changes in rx input come in at 9ms = 111Hz and influence the D-term based on setpoint rate which is calculated before the filter is applied, so that could explain why pt1 feels better than biquad.  

If we combine the filters on gyro with the filters on D, we get something like a 2-3 order filter but with weird behavior on magnitude below the cutoff value. (The coefficients should be changed to get a proper behavior when cascading filters.)
So there is not only a phase shift between P and D, but also a large difference in magnitude.  

Instead of disabling notch filters. I'd recommend raising the cutoff of the D low pass filter.  
By moving the pt1 cutoff higher and using notches, noise is reduced a lot without influencing lower frequencies.

As an example this is what I use on my super noisy noise-o-copter and get cool motors:   
gyro filter: biquad at 110Hz   
gyro notch1: 330, cut 250   
gyro notch2: 250, cut 170   
dterm: pt1 170Hz   
dterm notch: 280, cut 160    
![PT1 vs BiQuad](https://static.rcgroups.net/forums/attachments/6/5/0/4/8/6/a9947852-19-pt1-biquad.png)

All the filters and signal sources interact with each other and it's difficult to find the perfect setting.
Here's my point of view.

We have:  
` < 110Hz: Useful, important signals  `   
` > 200Hz: Motor noise / frame resonance / unwanted signals  `

Gyro filters:  
lpf, notch1, notch2

Dterm filters:  
lpfD, notchD


Signal path is like this:

`gyro -> lpf -> notch1 -> notch2 -> P term                   -> motor`  
`gyro -> lpf -> notch1 -> notch2 -> D term -> lpfD -> notchD -> motor`  

Each filter creates more delay the stronger it is.

We want to:
keep total delay as low as possible  
keep delay between P->motor and D->motor as low as possible  
filter out as much noise as possible   

lpf of second order (biquad) creates most delay but has a steep curve and does not influence frequencies below cutoff too much. 
lpf of first order (PT1) creates less delay than biquad, but has a strong influence on lower frequencies.  
Notches are great to remove certain frequencies created by motors and have a delay based on filter width but less than lpf. 

In my opinion it's best to use biquad and notches for gyro. This does a great job at removing noise but keeping the useful data.  
The dterm needs still additional filtering so a PT1 at high cutoff and another notch take care of noise without touching low frequency data. This also keeps the delay between D and P low.  

The default setting of a biquad for D with quite low cutoff creates a lot of delay and also lower magnitude of useful data.   

