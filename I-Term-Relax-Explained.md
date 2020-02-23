I term relax aims to inhibit **I** during fast manoeuvres by preventing it to further accumulate. Simply put I term relax will keep **I** constant during fast manoeuvres. 

I term is relax rely on setpoint or gyro (**iterm_relax_type**) to detect such manoeuvres and preventing I term to accumulate further.
To tune when I term accumulation should be suppressed a cutoff value (**iterm_relax_cutoff**) can be set.
The lower the cutoff value the earlier I term accumulation is suppressed.  

* In setpoint mode this suppression is based on the speed of the change in setpoint.
* In gyro mode this suppression is based on [TBD].

In the following log trace demonstrate how different cutoff value affect I term relax (I value in green and setpoint in red). 
![](https://user-images.githubusercontent.com/2025999/75109976-9a8f8900-5628-11ea-840c-eef16cfac4d4.png)

Notice that when set to 15, this is shortly after the sticks stop moving, and just about the perfect time.


## Freestyle Tune
For smooth flying such as freestyle when extremely fast and aggressive turns are rare I term can be relaxed as far as completely inhibit it during manoeuvres. Freestyle tuning might requires lower cutoff values, typically about 10-15. 

Ideally I term should be 'locked' as the moves starts and come back on shortly after the sticks stop moving as illustrated in the log trace below.

![](https://user-images.githubusercontent.com/2025999/75110071-ae87ba80-5629-11ea-8644-58072c21fa2d.jpg)

## Racing Tune
When racing shorter I term accumulation suppression allows the quad to better track aggressive turns and quick blips. Racing tuning might requires higher cutoff values, typically about 20-30. 
For example, in the above log trace a higher cutoff value allows for better setpoint tracking on quick twitch inputs.

![](https://user-images.githubusercontent.com/2025999/75110143-98c6c500-562a-11ea-980f-2753421dd824.jpg)




## Known Drawbacks

Bacause I is 'locked' to the pre-existing value before entering a fast manoeuvre, when it comes back on the old I offset takes a little while to resolve.

![](https://user-images.githubusercontent.com/2025999/75109721-61eeb000-5626-11ea-81ef-75bee6628d59.jpg)

Such effect is even more prominent with gyro mode, since the I term locking is slightly delayed compared to setpoint.

![](https://user-images.githubusercontent.com/2025999/75109705-25bb4f80-5626-11ea-925f-ad0f5fd54eb8.jpg)









