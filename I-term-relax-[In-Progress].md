# I Term Relax

I term relax aims to inhibit I during fast manoeuvres by preventing it to further accumulate. Simply put I term relax will keep I constant during fast manoeuvres. 

I term is relax rely on setpoint or gyro to detect such manoeuvres and preventing I term to accumulate further.
To tune when I term accumulation should be suppressed a cutoff value (**iterm_relax_cutoff**) can be tune.

* In setpoint mode this suppression is based on the speed of the change in setpoint.
* In gyro mode this suppression is based on [TBD].

The following 
![](https://user-images.githubusercontent.com/2025999/75106639-fead4f80-561e-11ea-9bf8-4f284556673a.jpg)

Notice that when set to 15, this is shortly after the sticks stop moving, and just about the perfect time.


Freestyle Tune

Notice that when set to 15, this is shortly after the sticks stop moving, and just about the perfect time.



Now the duration of I suppression is much shorter. It reqiures really fast inputs to suppress I accumulation. Consequently I is much more active, and changes more dynamically. It gets 'locked out' for shorter periods, and can get involved in more rapid reactions to fast inputs. This is a good thing for high authority quads. This is a high authority quad, so it is actually not so bad lol.
In this flip we see the same I accumulation mid-flip, but I gets 'locked out' for much less time, and aggressively returns to help control the error. P doesn't have so much to do. But because I is slower than P, it hangs around a bit too long, leading to the small overshoot just visible to the right of the roll finish.
This quad would turn tight corners far more accurately with iterm_relax_cutoff of 30 than at 15, but will have some bounce back at 30 that is not present at 15.

Racing Tune
I couldn't find a similar blip input to compare on lower iterm_relax_cutoff logs, but when testing out racer logs those kind of twitch inputs are what we use to fine tune them.
Freestylers hardly even what those inputs, so that's why iterm_relax_cutoff has to be different depending on authority and purpose.



## Known Drawbacks

Bacause I is 'locked' to pre-existing value before entering a fast manoeuvre, when it comes back on the old I offset takes a little while to resolve.

![](https://user-images.githubusercontent.com/2025999/75109205-8bf1a380-5621-11ea-8eb4-df746a8dbe5d.jpg)











