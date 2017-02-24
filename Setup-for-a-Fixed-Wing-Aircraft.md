Some have asked if BetaFlight can control a Fixed wing aircraft. 

###Boris' Note:  
####I would not trust this as nobody is testing it, but it may work.
####iNav fork puts a lot of effort in airplane modes. Its better to try them. 

####The page is to help those that wish to Experiment and Test Fixed Wing functionality with BetaFlight. 
####Do Remember that YOU are the TESTER.

Reference the [Betaflight resource remapping](https://github.com/betaflight/betaflight/wiki/Betaflight-resource-remapping).
Note: Pin re-mapping requires BetaFlight 3.1 an higher.  

####AresFPV asked this in Boris' ßF Thread and then did research on how to do this. His findings are: 

Fixed wing success:  
For the few of you that are trying to get fixed wing working in BetaFlight, I've been able to have some success on the bench. I'm using BetaFlight because iNAV doesn't have the piko blx target yet. This post helped a lot: https://www.rcgroups.com/forums/showpost.php?p=36886595&postcount=45058  
 
Select WING as type.  

Basically you need to map the right servos to the pins used by motors 3 and 4 (on a wing) using the CLI Resource.   
Use the servos tab to reverse them if needed.   
Set the PWM frequency as separate from PID loop and set it to 50hz to make the servos work (you will lose all of the new ESC protocols but that's not as important on a plane).   
The modes and mixes work fine for me as-is and I expect that everything else will be fine as well(like OSD). I just need to add FPV to it and get it up in the air! 

####A Post by RCvehicleGuy:  
Well I messed with trying to get betaflight to work with an airplane. It was clearly not intended to work.  
What I wanted to do was use the resource command to put servos 1-4 on motor outputs 1-4 on my naze board and do a custom smix to assign stabilized roll, stabilized elevator, rc throttle, and stabilized rudder to the outputs respectively. Like a futaba radio. It sounds simple but for the life of me couldn't get it to work last night. I think something is broken somewhere, these possibilities haven't been tested I would think, seeing as betaflight is designed for acro-quad flying it really only gets tested on acro-quads.  
I stepped the PID-loop frequency to .06 khz. I am running digital servos so theoretically I could do .3 khz and update them at 300 hz, but going analog speeds just to see if it works.  
I think the key is the smix command, either it isn't working correctly or I don't understand it well enough to work. I'll try again when I have more time and see if I can't get this working.  
so to output stabilized roll to servo 1 I believe the smix command is, if you just reset the smix:  
smix 0 0 0 100 0 0 100 0, or something like that. rule 0, servo 0 (1), source 0 (PID roll), 100 (rate), 0 (null speed), 0 (min), 100 (max), 0 (box. don't know what this is).  
As I said I will try again. I already tried iNav but without the resource command I can't map servo 1 to motor output 1, or don't understand how that would work at least.   

Part 2:  
OK! I'd like to update that I have the outputs on my Naze32 working for stabilized roll, pitch, and yaw, good for a typical 3 servo 4 channel plane.
I am using a 1k cycle and pid loop time. It was the .06 pid loop that was messing up the smix. I thought that the pid loop sent synced values to the escs, this is obviously not the case. So much to learn. I wanted to use motor output 4, resource b07, as rudder, but it seems broken on my Naze so I moved that to 3 and decided to just use the throttle output on my receiver instead.
It's interesting to watch the PIDs wind-up with throttle applied. They wind up up much harder for setpoint changes and hardly at all for board movements. This should make for some interesting flight characteristics. I'm excited to get this on the Crack Laser and in the air to see how it changes the way it flies. It will be funny to be doing a rolling loop and let go of the aileron and have iterm windup keep it rolling another 3 revs or so, lol.

I'm more interested in the applications of level mode. For people new to airplanes or for strangers who ask if they can fly, having a working level mode means hey why not. On small 3d foamies at least, what is usually a touchy aerobatic machine could be turned into what amounts to a 3 channel trainer with level mode working.

I'll update again after I get it installed in the plane, reversing figured out, and a testflight in.
Here is a screenshot of my smix: 
https://www.rcgroups.com/forums/showthread.php?2464844-Betaflight-Flight-Controller-Firmware-Discussion-Thread/page3040#post36958637  

`# smix    
`smix 0 2 0 100 0 0 100 0    
`smix 1 3 1 100 0 0 100 0    
`smix 2 4 2 100 0 0 100 0    
`smix 3 5 7 100 0 0 100 0    

Part 3:  
Well the thing about it continuing a roll for three revs after you finish a rolling maneuver is true, lol. I've set the I-terms to 0. My original understanding was that there is I term decay and I term gradually becomes less and less effective as it wound up and built up error would be forgiven after a while.  
Will try to tune some more. I did put the gyro on a perfectly flying plane, might be impossible to make it any better.   
Part 4:  
Well I've come to the conclusion that airplanes just don't need gyro stabilization, I'll keep playing with it, because it's interesting, the plane does fly. So far the only neat thing it has accomplished are hard stops for point rolls.
I have D at 3 right now and it still has too much of a dampening effect, As you approach your desired rate it steps in and backs off the rate for you. Putting D at 0 and upping rates a little.   
After turning up the rates and getting rid of D it's starting to make sense. Going to lower my P on ailerons and introduce D back in there and see where it's at. Flies kind of weird, I guess we'll see if it works out.   
