## Changes in Default Filter settings
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

### From ctzsnooze's posts:  
The ideal value for filters is as high as you can go without getting hot motors etc. I don't think anyone can tell you how high you can go.

But your particular concern was that your motors still felt a bit warm. So I'd map out what dterm cut does while leaving gyro cut where it is. The only way to find out is to do the testing. Once you know the best value for Dterm cut, ie the value where motors are coolest, keep dterm cut and gyro cut in the same proportion from then on. If, despite finding the coolest value for dterm cut, they are still warmer than you'd like, try say reducing both by 20%. If they are cool and you want crisper performance, try increasing both by 20%, and keep going up until they become clearly warmer than with a lower value.

If you really want to optimise things you need blackbox. Your motors might just be warm because you fly hard! :-) There may be no PID oscillation at all. If that's the case, and you don't have blackbox, you can keep going up on the filters, both in a fixed proportion, keeping dterm cut above gyro cut to the same percentage - until the motors get obviously hotter or noisier, then back off a step. Then if you could be bothered, repeat the dterm cut optimisation until you have the filters set as best they can be.

Note that if you change P or D upwards or change props you may have to modify the settings.

On my setups 70 for gyro and 90 for dterm keep motors cool even with the most beat up props, flexy frames and oldest motors. 

See [BB logging page](https://github.com/betaflight/betaflight/wiki/Black-Box-logging-and-usage) for measuring noise and filter.

The sequence in disabling notch filters isn't certain.

The best approach is to blackbox the quad with both gyro notch filters off and look at the spectrum of the gyro trace. If there is a big peak there, and that peak is resulting in noise that is visible in the motors trace, then it should be filtered out by a gyro notch. If there is diffusely too much gyro noise, but no specific peak, then either use the biquad fitler or lower the gyro cut low pass filter.

Not removing a prominent spike in gyro noise will just not be a good idea.

Once the gyro trace is relatively clean, then look at Dterm noise spectrum to decide how much filtering it needs; if it has a spike that is causing problems, it should have a notch. But if you already have controlled the same spike in the gyro trace, you may need no notch on D.

If you can control a spike with a notch in either the gyro trace or the D trace, which approach is better?

Well, it depends. A big spike in gyro that is not filtered out will cause noise on the P trace that will get into the motors. Notches on D will not get this out of P. The only way is a notch on gyro. On the other hand, if the spike is not very prominent in the gyro spectrum, but Dterm exaggerates it and makes it a problem only in D, then put the notch in D and don't worry about a gyro notch for that particular problem.

Without a black box there is no reliable way to know what to do. Certainly you can just disable the notches randomly, listen to the sound of the motors, see that they don't get hot, etc, and keep only the filters that are needed. But as to setting the ideal values, you need to get a black box log for that.   

Recently helped review a noisy log. Quad had PT1 on both gyro and D, and only one single notch, on Dterm.   
Log showed *no* peak in noise at the centre point of the Dterm notch when looking at debug notch or the incoming gyro data! That notch was achieving absolutely nothing! :-)

Take home message: you cannot guess where to put notch filters!   
Notch filters really need a black box log to get them focused exactly on a peak. Otherwise they are useless and counterproductive.   
If you are going to blindly remove a notch to 'see how it goes', try removing the *lowest* frequency notch before any others.   
That's the one that is most likely to mess with frequencies relevant to flight dynamics.  

A notch filter set very high up (eg 300 centre 200 cut) will have much less effect on flight dynamics than a lower notch filter (eg 200 centre 140 cut). 

Did a heap of testing today... interesting stuff.

I realized that if you put P, I and D all to 2 (ie, almost totally off), what you the blackbox the gyro trace (quad safely hand held or in a restraint) shows gyro data without amplification by PIDs. If you zoom in on gyro (say 1000 times, with no expo on the trace), you can see the 'input' noise as would go into the PID loop.

If we then add P alone, and test while (safely) hand held, it becomes apparent that the amount of noise added by the 'P' part of the PID calculations is small. Even with PT1 and no notch filters, P itself doesn't seem to be much affected by motor noise, nor contribute to feedback resonance in a big way. If it wasn't for D, all we'd need is a PT1 on gyro.

But when you add D - that's when it gets interesting. With only a PT1 on gyro, D carries a heap of amplified noise back to the motors - even if D lowpass is a biquad.

This is a cunning way to identify what P or D individually add to the noise sent back to the motors.

Overall I get the impression that we need more D filtering than Gyro filtering. We can drop back on gyro, but when we do, more noise gets through to D, making a worse overall noise picture. A but more D filtering would allow less gyro filtering, and probably better handling.

PS - I too had many occurrences of 'prop resonance' where the frame shakes and the prop blades flex like crazy, starting really abruptly at just above idle throttle, and easing off with more throttle. It starts abruptly, sounds really bad, the quad shakes violently, and if you're on the ground it adds thrust and tries to fly away. If you throttle past it, all comes good again. Motors quickly get hot! I recorded it. In my case it was a very narrow peak at exactly 145 Hz, put a single gyro notch filter there and it disappeared. Interestingly this shaking was entirely in pitch and roll, yaw was flat and smooth. Also the D component was three times larger than the P component. Once a carefully placed notch filter eliminated the problem, flight was entirely normal all the time. I don't really understand what's causing this, it is definitely a feedback process driven mostly by D, but why it should be so powerful at such low rpm has me beat. 

[NO MORE PROPWASH OSCILLATION ](https://www.youtube.com/watch?v=PHkofDq_JxU) A Joshua Bardwell Video.

## Filters were changed in Version 3.0 

### What Defaults changed in 3.1.7?
Boris's answer:  
Basically people kept complaining that betaflight default D was too conservative and therefore changed.  
Setpoint transition has been disabled (1.0) to give more linearity over the entire stick.  
I still recommend that you slowly remove default filtering as well if your setup allows you for best results. Nowadays most do softmounting so removing of filters can easily improve performance. The defaults are optimized for hard mounted medium noisy environment for safety. The best tuning performance is achieved with as less Du.

#### I think best filter removal steps would be.  
set d_lowpass_type = PT1 (in the CLI) should always be done first and I think possible on every setup.  
If it is still fine than:  
Remove notch 1  (set Freq = 0 in the config GUI or CLI to disable Notch filter)  
Then if still fine remove notch 2   
Now if your setup is still clean you could even remove dterm notch.  
That will give you the best results. Sharpest response, easiest tuning and literally no prop wash on good setups.  

Note that default betaflight filters are made so every beginner can put a quad in air without burning his gear.   

Dterm lowpass is always needed. Never remove that! Even on the cleanest setup.

I think if you really put some effort to build a clean setup you easily can get away without most of other filter settings.
To test that I was able to repeat it on 3 of my builds.  Hardware softmounting is always filtering without extra penalty.  

The most difference with and with filtering comes from the steep gyro changes. The more filtering there is the slower P will react on high gyro changes and translates itself to more prop wash and less direct feel.  
As I said defaults are meant to be safe and protect against most common vibrations shown across many hardmounted 4 / 5inch setups. But more and more people care about clean setup nowadays than lets say 1 year ago and that means you may get much better running setup with some filter removals. 

#### question from linklemming:   
From BB analysis with debug notch on, I have determined that D lowpass of PT1 is fine and that I only really 1 notch filter (300Hz Center, 200Hz low). I have a softmounted FC.  
I can see the spike in the frequency range both in P and D.  

Boris mentions removing gyro notches first, I'm wondering what the trade-offs would be in selecting gyro notch vs D notch. Obviously if using only gyro notch, I get noise reductions in P and D and it seems to me this is better than using D notch unless there is some flight behaviors that would be better suited to using D notch only. The noise I believe is low enough that any P noise from using just D notch would be minimal.  

Interestingly all the quads I did testing on this weekend show the same noise spike around 300Hz (+/- 30Hz). 
This includes:   
F1-4B 3mm with DYS 1806-2700kv motors, dal 4045V2 tri blades on 4s  
My own design sub250g 180 quad(3mm) with rcx1306-3100kv, dal 4045 dual blades on 4s  
My own design sub250g 150 quad(3mm) with dys1306-4000kv, dal 4045 dual blades cut to 3" on 4s  

Im guessing this is just frame resonance? All are 3mm single baseplate designs.   

Answer from ctzsnooze:   
To be honest, no-one really knows.  

If you compare D trace (zoom in, smoothing off, expo off) to P trace (same zoom), and if most of the noise is in D, then there are two ways of dealing with it.  
First way of thinking about it, just filter what is bad - ie just filter D only. This fixes the noise problem and delays P the least.  
Second, put the notch on gyro, that's where the noise is coming, this way both P and D will have less noise, and the relative phase lag between P and D won't change.  

Frankly I doubt it makes any difference..   
I do like the idea of only adding filtering where it is necessary; in this case, just putting the notch on D. Sure there will be slightly more phase delay between P and D, but D is already phase delayed because of the D lowpass filter, and the extra phase delay from a 300/200 notch filter on D alone may have no adverse effect.  
If you had time to try both and compare logs, that would be great. If possible make your flight testing 'blind', ie get someone else to make the change in the firmware so you don't know which you are flying.  
Be surprised if it makes the slightest difference. If you had to enable the notch filter lower down, eg centre 145 cut 90, for example, it might matter a lot. But high up probably either way is fine, gyro should end up slightly smoother and fly just as well.   

#### Testimonials   

Redtail2426:  
So I ended up switching to pt1, removing both notch filters, and removed the d term notch filter, and the prop wash is pretty much 99% gone, much much better than it was with the default 3.1.7 filtering. Awesome job guys  

kevinpratt823:  
I got rid of the Dterm notch and went PT1 yesterday as well, and it was a considerable improvement on a high power to weight build that was struggling with that last bit of oscillation, opened my tuning window up, raised PIDs a bit.   

Hattrickmwf:   
Going to PT1 and removing all the notch filters made a huge difference in my quad too. Almost Zero prop wash. I also noticed it handles impacts with gates or ground touches much better too. No Tasmanian Devil quad after impact. May be placebo effect, but it seems to handle a lot better too.  

Cheredanine:   
Some interesting stuff:   
I removed filters on f3 flight controller, by the time I removed first notch filter flight performance was much improved (perceptively) when I removed second notch didn't make much difference (or none that I noticed subjectively) although bb gyro traces remained clean, clearly there was a sort of milestone I passed in terms of latency.  

I repeated on an almost identical quad running f4, this is where it got interesting, the improvement was similar but les marked, subjectively the most noticeable thing was prop wash vanished.  
BUT,  
On a whim I switched from 8khz/4khz to 32khz/16khz the change in speed had a dramatic effect, gyro traces were noisy as hell, but motor noise was at values like 400kz which 8khz should find easily. Front two motors got warm and audibly screamed over about 15-20% throttle. Quad actually flew quite well.  
Got both set of bb logs and not particularly worried as I usually fly8/4 but thought the result was quite interesting.  
  
Oz asked:  
You find 8/4 flies better than 32/16? (Properly soft mounted)  
Cheredanine's answer:  
Subjective but I don't find improvement over 8/4, probably less than that. I have 5 quads with bluejay f4s in them, i gave 32/16 a good test since it became available. Provided you tune it properly, 8/4 is as good as 32/16 as far as I can tell, I am 100% sure I would not be able to tell the difference in a blind test. (The quads I mentioned above are both dquad obsessions with returner r4, 25a blheli_s 16.6 escs, one uses an rg-ssd, the other has a bluejay., no noticeable difference in flight apart from:  
Normally (with filters) 32/16khz has motor noise above 4khz, this is not apparent on spectrographs at 8/4 but then given the scale of the graph and Nyquist limit I wouldn't expect it to.  
But strip back the filtering and 32/16 is far messier, flies ok, but the heat and noise from the motor was worrying.   
All quads have vibration isolation on the fc, where possible using rubber standoffs (exceptions being shrike and tokio-x prototype, the former has to use rubber o rings and the latter has a unique vibrations isolation mount).   
32/16 has so far not flown any better than 8/4 and in some cases flown considerably worse.  

Boris provided the following info:  
There is additional filtering present when running at 8khz gyro or lower, this is not present when running 32khz. Therefore changing d low-pass to pt1 and removing notch filters when running 32khz will produce much messier gyros.  
In summary I found quad flew fantastic on 8khz gyro with filters stripped back but on 32khz gryo I had to leave them in place to get similar performance and no heat in motors.  

Posted by QuadMcFly:  
So here's my experience with PT1 and the notch filters, as well as some other general thoughts. I gave RF1 a shot for about a month, because I like to try things so I can actually speak from experience. Something about my rig really hated RF1. I had some pretty significant issues with low throttle noise, and the stick handling never felt right. Kind of felt unpredictable like I was having to fight it instead of flowing with it. I don't necessarily think that is reflective of everyone's experience on RF1, I just think there are some setups that it doesn't like at all, and mine happened to be one of them. Anyway I got tired of fighting it, so I put BF 3.1.7 on the revolt, 8k/8k PT1 Dterm, lower gyro notch removed. Got it flying a few days ago and I have been totally blown away. The difference was stunning. Like going from PPM to S.Bus all over again. I don't think I've ever flown anything that flew this well, and I've flown a lot of stuff! It still needs a bit of adjustment to clean up that last bit of prop wash, but man alive! Amazing for a totally blind tune, just thrown in based on the power train. The stick feel is utterly amazing in terms of precision and crispness.  

As I mentioned a few days ago, DO NOT run PT1 dterm on 32khz! It is quite likely to launch your quad at full throttle! Found that out on my first attempt 8k/8k on the other hand is absolutely stunning! 10/10 would recommend!  

Here's a boring few clips from about the only spot I really get a chance to fly, feel free to skip it:  
https://youtu.be/3KZlI8F0ER8  

Boris' response:  
Looking great indeed!

It basically all comes down to filter interaction. There is no 32k magic at all.  
Any kind of bench test didn't show any performance gain going to 32khz update speed for motors.  
But 32k mode fully disables any kind of internal gyro filtering, while 8k mode still has a 256hz lo-wpass active.  
The 256hz low-pass is kind of just not enough filtering by itself so it needs a tiny bit of software assistance like pt1 ans you get the best balance of filtering without much signal damage.  
32k mode, which essentially only has internal 3600hz low-pass active needs a lot of software assistance to make it useful.
The betaflight philosophy has been more to make every-bodies quad fly without damaging their gear and that meant filter protection against any common oscillation scenarios.  
So with all default soft filters on top of it you were still able to fly 32k mode and it felt a bit better than 8k modes as there was simply less filtering.   
It is not just delays we are here talking about. What is more important is the amount of signal deformation on sharp changes like what you can get in propwash cases or other very quick changing frequencies.  

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

gyro -> lpf -> notch1 -> notch2 -> P term                   -> motor  
gyro -> lpf -> notch1 -> notch2 -> D term -> lpfD -> notchD -> motor  

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

post by ctzsnooze on Evaluating and setting Filter:  
Summary:  
- Raw gyro spectrum shows what is happening without any filtering at all.   
- Normal gyro spectrum shows what happens after gyro LPF and the gyro notch filters.  
- Spectrum from D trace shows how the D calculation adds noise on top of the filtered gyro data. To see what D is 'adding', we compare the spectrum from the D trace (in any given axis) with the gyro spectrum on that axis. We look to see what the D calculation 'adds on'. It usually amplifies noise, progressively more at higher frequencies. That's why D MUST have at least a PT1 low pass filter, and why if there is any high frequency peak creeping through gyro, it will be even bigger in D. The D notch should be applied to that final remaining notch after your gyro notches, or if you have no gyro notches, to deal with whatever is the biggest notch getting through the gyro filtering into D. That's why I say, *always, always always get the gyro filters right first, then look at the D filters*.   
- finally, PIDsum and motor trace spectrums shows what the motors finally receive; this is the sum of the gyro->P and gyro->D pathways added together and modified by your P and D weightings.  

Remember that we are not 'trying to fix a spectrum'.  
Our goal is to reduce the relative amount of noise in the signal going to the motors - say as a percentage of total signal to the motors - to a reasonable level.

Therefore the final step is to visually examine the motors trace (with scaling at 100% and no expo in the blackbox config), and check that overall the noise contribution is small. It does not have to be zero, just small enough to not be more than say a few percent, ideally. 5% noise costs 5% power and gives you 5% heat. The less noise, the better, all things being equal.

You can look at the motors spectrum to see, if there remains a bit more noise present than you want, what its frequency range is. Then you can go back through the pathway to find where best to deal with it.

The purpose of the spectrum and the filtering options is to identify the frequencies that contribute to the noise, so that you can then then to set the cut points on the LPF's to get rid of most of it, then add, if needed, the notch filters to remove any stubborn remaining peaks. But you don't need to put a notch on a peak if the actual amount of noise from that peak that is actually present on the motor trace - the waveform itself - is so small as to be irrelevant. We aren't fixing a spectrum!

Note that as you reduce filtering, the quad will become 'twitchier'. Reducing filtering means that P and D now are active in higher frequencies that they never 'saw' before. So the quad may fly a bit better perhaps, but may also resonate or go insane from feedback when it didn't used to.

The whole goal of filtering is to allow more P and more D than you other wise could use without filtering. And we want more P and more D, if possible, because they are good for propwash. We want the highest P and D possible without overheating motors.

But, reducing filtering may limit how much P and D you can safely run without overheating.

It's just not true to suggest that 'less filtering means better performance', especially if it limits how much P and D you can run. If you have beat up old machines and dodgy props, you will be able to safely run them on quite high P and D with solid filtering.

Somewhere there is a good balance between too much and not enough filtering. Finding the balance that works best for you is what its about.


### Filter Delays

Post by ctzsnooze  
All filters add delay. Doubling slope on an IIR LPF doubles delay since the same 1st order filter is simply applied twice. None currently are FIR. FIR were evaluated and not as good as simple IIR. Dterm is IIR, gyro cut was biquad (i think it still is). There is a recent post about the notch filter that linked to the GitHub page where the Notch was discussed before implementation. Diagrams there show delay for different filter combinations. Lots of thought has gone into current filter design.  

Posted by ctzsnooze:  
Switching from biquad to PT1 , while leaving frequency the same, say at 100Hz, will do the following:  
Twice as much noise at 200hz  
Four times as much noise at 400hz  
Almost no change in phase delay at 10hz  
About 15 degrees *less* phase delay at 50hz  
45 degrees *less* phase delay at 100hz  

The improvement in phase delay at 100hz can be thought of as about 1.5ms less absolute delay.   

Post by r.a.v. showing Filter delays:  
Here's an overview of the delay between 90Hz and 170Hz:  
biquad: 2.75ms vs 2.5ms at 100Hz  
pt1: 0.88ms vs 0.8ms   
![BiQuad Delay verse Cut-off frequency](https://redirect.viglink.com/?format=go&jsonp=vglnk_149199926656316&key=8a24c98a696b4e5723db293f62190b87&libId=j1exth4b010004o4000DAkk8ce2y8&loc=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3162&v=1&out=https%3A%2F%2Fstatic.rcgroups.net%2Fforums%2Fattachments%2F6%2F5%2F0%2F4%2F8%2F6%2Fa9950155-145-phasedelayBiquad90-170.png&ref=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3161&title=Betaflight%20Flight%20Controller%20Firmware%20Discussion%20Thread%20-%20Page%203162%20-%20RC%20Groups&txt=)

![PT1 Delay verse Cut-off frequency](https://redirect.viglink.com/?format=go&jsonp=vglnk_149199932789918&key=8a24c98a696b4e5723db293f62190b87&libId=j1exth4b010004o4000DAkk8ce2y8&loc=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3162&v=1&out=https%3A%2F%2Fstatic.rcgroups.net%2Fforums%2Fattachments%2F6%2F5%2F0%2F4%2F8%2F6%2Fa9950156-96-phasedelayPT190-170.png&ref=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3161&title=Betaflight%20Flight%20Controller%20Firmware%20Discussion%20Thread%20-%20Page%203162%20-%20RC%20Groups&txt=)  

I'll just throw in some notch delays for notches with cutoff 80Hz below each center.  
As you can see the delay is very low but the strong filtering allows higher cutoff for lpf filters.  
So total delay of notch + high cutoff lpf is lower than lpf alone with low cutoff while the noise is still reduced significantly.   

![Notch Delay verse Cut-off frequency](https://redirect.viglink.com/?format=go&jsonp=vglnk_149199948127320&key=8a24c98a696b4e5723db293f62190b87&libId=j1exth4b010004o4000DAkk8ce2y8&loc=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3162&v=1&out=https%3A%2F%2Fstatic.rcgroups.net%2Fforums%2Fattachments%2F6%2F5%2F0%2F4%2F8%2F6%2Fa9950217-164-notch220Hz-300Hz-80cut.png&ref=https%3A%2F%2Fwww.rcgroups.com%2Fforums%2Fshowthread.php%3F2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread%2Fpage3161&title=Betaflight%20Flight%20Controller%20Firmware%20Discussion%20Thread%20-%20Page%203162%20-%20RC%20Groups&txt=)  

