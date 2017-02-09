To be added to

###Examples

####Port Forwarding   Note: not complete   
SP3 FC, Radio's AUX 2 as Servo 0.  
Using IO_1[4] (RC CH2, I presume)
(1) Make sure you are not using PPM input (CH2 shares a timer with PPM).  
(2) Use following CLI commands:  
`resource pwm 2 none`  
`resource servo 1 a1`  
`save`