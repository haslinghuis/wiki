##BetaFlight 3.0  
3.0 is currently still in Alpha testing. This version has many changes in the under laying code so has its own section.  

Plan here to to list and explain only the differences from the previous 2.x version.

From Boris for those just trying 3.0:  
Just use things available in the configurator and leave the rest on default.

Betaflight has 2 different goals.
1) have super stable firmware with solid defaults to just be able to fly when you want

2) from scientific point of view it is good to keep improving and introduce new features where those who like to experiment can play with ans give valuable feedback. These are mostly burried in the cli   

###New CLI commands
Note that most are better to set using the new BetaFlight Confug GUI.  

####'''set pid_tolerance_band''' = ?<br />
<i>[?..?]</i><br />
Reduces "hunting" effect from pid controller. 
What does the pid controller do? It hunts for error all the time. Its mainly P and D what are the quickest ones. The problem is that when error is very small like in forward flight or hover where not much error needs to be corrected the pid controller gets more "relaxed" to not keep looking for perfection. The amount of pid relaxation is determined in percentage in pid_tolerance_band_min_reduction.
You can for example remove yaw noise on this way till certain level, but you may need to retune.  

####'''diff'''<br />  
to see what differs from default  

