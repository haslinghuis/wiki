Some have asked if BetaFlight can control a Fixed wing aircraft. This Wiki Page should have all the info needed here.

Reference the [Betaflight resource remapping](https://github.com/betaflight/betaflight/wiki/Betaflight-resource-remapping).
Note: Pin re-mapping requires BetaFlight 3.1 an higher.  

AresFPV asked this in Boris' ßF Thread and then did research on how to do this. His findings are: 

Fixed wing success:  
For the few of you that are trying to get fixed wing working in BetaFlight, I've been able to have some success on the bench. I'm using BetaFlight because iNAV doesn't have the piko blx target yet. This post helped a lot: https://www.rcgroups.com/forums/showpost.php?p=36886595&postcount=45058  
 
Select WING as type.  

Basically you need to map the right servos to the pins used by motors 3 and 4 (on a wing) using the CLI Resource.   
Use the servos tab to reverse them if needed.   
Set the PWM frequency as separate from PID loop and set it to 50hz to make the servos work (you will lose all of the new ESC protocols but that's not as important on a plane).   
The modes and mixes work fine for me as-is and I expect that everything else will be fine as well(like OSD). I just need to add FPV to it and get it up in the air! 

