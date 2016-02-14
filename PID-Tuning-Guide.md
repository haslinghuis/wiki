##How should I tune my Copter ?

1. It's recommended that these step be done in Acro mode even if you are usually a Level/Horizon flyer.  Example PID values shown below correspond to the Rewrite PID controller (PID controller #1)

2. Start with slightly lower than default P gains as provided by the installed BetaFlight firmware. P of 4.0 on Pitch and Roll are good starting points. Also lower the I and D gains on pitch and roll in order to tune P with minimal interference from I and D. I of 20 and D of 5 are good starting points. For yaw, it is prudent to decrease default P by HALF and reduce I just a bit, to eliminate that axis as a source of oscillations.  

3. Over a series of flights, increase P gain on Roll axis until you see oscillations when you approach full throttle and you get very rapid visible and audible shakes. Then set P term to roughly 70% of the value that caused the oscillations. 

4. Repeat step 3 for Pitch axis.

5. Test to see if the quad holds the desired roll angle and does not drift by rolling the copter to a specific angle, and then punch and drop throttle several times.  The angle you gave it relative to the horizon should not change significantly.  If the angle appears to drift, increase I gain.  If you don't see drift, don't change I.  You can change the "feel" of your copter by raising or lowering I after you achieve a good tune. (I does not really affect final P and D values.

6. Repeat step 5 for Pitch axis.

7. Increase D gain on each axis ONLY to the extent that it helps reduce bounceback after flips/rolls or prop-wash oscillations after an abrupt descent. If neither is a problem, then LEAVE D LOW.  At this point the Copter should be around 80-90% tuned. 

8. Yaw often requires the least tuning, but it may still introduce significant oscillation if you ignore it.  Start with the Yaw P that you chopped in half in step one and verify that you do not get significant vibrations when you do a long punch-out or fast forward flight.  Start pushing up Yaw P by .5 increments until you start to see roughness through your fpv camera when in fast forward flight or punches.  Then decrease a bit.  Fine tune by looking at Yaw P term in blackbox.  It MAY be oscillating a bit, but pull up the Yaw gyro trace to see if those P oscillations actually make it to the Gyro. If the yaw gyro looks relatively smooth, you're ok. 

Note: Because yaw inherently has less positive control (a.k.a. authority), than pitch and roll, a wider range of values are acceptable.  Relatively higher P and I values and relatively low D values are the norm because of the inherent lack of authority compared to pitch and roll. A blackbox log is usually necessary to fine-tune. Most excess P oscillation comes from either roll or pitch, but if any roughness at full throttle remains, look at a blackbox log to see if yaw P starts to oscillate on full throttle. If so, decrease yaw P. 

9. Finally, refine the relationship between P and I by looking for a tendency to resist or "fall into" strong turns. Very low I values will result in an axis that drifts over time.  Low I values on an axis will allow that axis to change attitude more freely but may still hold attitude.  Higher I values on an axis will hold attitude very well, but may tend to resist movement and can add a  feeling of inertia.  Very high I values can create an overly "robotic" feeling and even oscillations.  Can also refine P by analyzing Blackbox Logs. This may get you closer to a perfect tune.

10. Once tuning is complete in Acro mode then move onto adjusted the Level/Horizon parameters to suit your flying style if you plan to fly in angle or horizon mode.

Remember not to get too carried away trying to get the BlackBox traces to be as clean as possible. If the copter flies really well and suits your needs then just get out there and fly !

** Other Notes:** 

When you look for high P term oscillation in a BB log, they do not generally look like wide sweeping arcs or jagged peaks and valleys. High P term oscillations manifest first at the very top of the throttle range and look like tight sine waves.  When these show up on BB logs, they may not always be detectable by sight or sound. By the time you can see/hear them, they are super-apparent on a BB log.  That's why the recommendation for initial tuning by sight and sound is to reach the point of visible/auditory oscillation and then drop back to 70% of that value.

The undesirable flight characteristic called bounce-back oscillation occurs when you abruptly return the pitch/roll stick to center, and the copter rotation does not make a "clean" stop.  It could be the result of:

1. D that is too low

2. P that is too high

3. or even P that is too low (a low P gain can cause slow, sloppy oscillations because it's not providing enough  authority to get to the intended end-state).

**More info:**
I term is usually not active enough to cause trouble, and can usually get it roughly tuned in pretty quickly.
But the D term can vary significantly depending on many different factors, and its amplification effect means that if D term is bad, it can be very bad, and in odd and unpredictable ways, depending on how noise is presenting itself and how the P term is acting.