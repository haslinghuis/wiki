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