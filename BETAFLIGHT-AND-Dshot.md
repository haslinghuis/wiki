# Dshot & BetaFlight   
Dshot is new and currently code is being developed and tested. Since there are many dependencies and not all current hardware works this is to help keep track.

The Official Thread for Dshot is: https://www.rcgroups.com/forums/showthread.php?t=2756129   
Please Read this thread for details of how it works and up to date info. The First Post of this thread has all the needed Links to code and other info. A good article on Dshot and ESC protocols is here:   
https://blck.mn/2016/11/dshot-the-new-kid-on-the-block/  

In the Dshot thread is this post on how to re-Flash ESCs & FC:   
https://www.rcgroups.com/forums/showpost.php?p=36200950&postcount=1468   

For KISS FC's and ESCs see the KISS Dshot thread:  
https://www.rcgroups.com/forums/showthread.php?t=2780055

BetaFlight V3.1 development code for Dshot can be found here:   
http://andwho.sytes.net:8080/job/BorisB_BetaFlight/   
Just remember that this is very Experimental code and may have serious limitations.

Some known limitations are (
Note: these may or may not affect all working targets):
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass-through.    
Note: should be fixed on most Targets but not all targets yet.   
- Some ESCs that have a signal filter cap may not work until the cap is removed. 
- 3D mode not yet working, developer's plan is to get the basics working well the get 3D working.
- A bug affecting Spektrum Sats, shows 988 on channels in receiver tab of Betaflight (soon to be looked at).
   Fixed on SPRF3 & REVO, may be fixed on other targets.
- PPM not working on some FC targets (limited testing due to most users using Sbus or Spektrum)   
   Note: Some targets need motor re-mapped to PPM pin so then PPM can not be used.
- LEDs not working or must be disabled to have all motors working due the DMA mapping conflicts.  
- The 'target.c' source files show that only Quad copters are supported in most targets and some targets support Hex copters.
- Some targets do not work if a Custom mixer is enabled. Instead use the new "resource" CLI command instead. Just be aware that not all outputs (pins) can be assigned a DMA channel for Dshot.  
- When BB logging works it appears that the actual Dshot ESC values are sent to the BB logger. The BB log Viewer then can not show motor values below 1000. A new BB log Viewer should be released when 3.1 is released.  

####A quick way to determine IF the hex flashed supports Dshot:  
Go to the CLI and type "get pwm". All settings with 'pwm' in the name will be shown with all options.
If DSHOT150, DSHOT300, DSHOT600 is NOT in the list for the "motor_pwm_protocol" then this firmware does NOT support Dshot.
Example from built 668 for MotoLab that does Not support Dshot: 

motor_pwm_protocol = ONESHOT42
Allowed values: OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED

####Setting Min Throttle with Dshot   
Dshot now uses the CLI command "digital_idle_percent" and the old 'min_throttle' setting is ignored.
Do type "get idle" to see if this exists and what the valid values are. This is a percentage of throttle used in armed state. The pid controller will never push use motor output below this percentage. For example if your min throttle when using multishot was 1040 and your total throttle was 1000 (2000-1000) then you should set your digital idle offset to 4.0 as 40 is 4 percent of 1000. You can play with these values a little but if you experience motor/prop stalls during flight this number should be increased as having this value too low could be the cause of that behaviour.

####ESC Cal and min/max throttle
ßF firmware with Dshot does Not use the min_throttle or max_throttle setting, these are ignored.
Just ensure that in the ESCs (BLHeli Suite) that PPM_MIN_THROTTLE is set to 1000 and PPM_MAX_THROTTLE is set to 2000.
Note: This should not be needed in BLHeli_S 16.43 since the PPM_MIN & MAX values are not used for Dshot.   

####Max ESC update speed supported by different dshot versions:
- dshot150: 8kHz max
- dshot300: 16kHz max
- dshot600: 32kHz max

Note: When DSHOT is enabled Unsyced PWM is disabled. DSHOT always runs at the PID loop rate.
####Dshot Values:  
0 = disarmed.  
1 to 47 = Reserved for special commands.  
48 to 2047 = Active throttle control.  

###Starting with a post from the Dshot thread:

Scouring the BF and Dshot threads has resulted in the following lists. The information posted herein is all provided on an AS-IS basis.
It has been derived from posts on this forum (RCGroups/BEtaflight & DSHOT threads), and direct correspondences with users.

USE THE FOLLOWING INFORMATION AT YOUR OWN RISK

BETAFLIGHT AND DSHOT -

Betaflight may not be able to support all Targets ( or specific boards) due to the design of each one differing on the DMA pinout. Look below to find what may or may not be working.

DSHOT on BLHELI_S ESCs will likely only support Dshot150 & 300.
Here is why: https://www.rcgroups.com/forums/showpost.php?p=36025232&postcount=376   
Now found that some BLHeli_S ESC will run on DSHOT600 with modifications. See list below for details.  
BLHeli Rev16.5 posted on github (27Nov2016). Dshot150, Dshot300 and Dshot600 are now supported officially.  

###Flight Controllers Tested to Support Dshot on Betaflight 3.1 Alphas:
- Airbot F3 (SPRACINGF3)  
- BETAFLIGHTF3  
- BLUEJAYF4
- Colibri Race v2.0  (build #722)
- Dodo (RMDO)  
- Dodo (SPRACINGF3)
- DTFc - Build #389 - working on the bench
- DTFC (DOGE) (build 682)
- FLIP32F4
- FURY F3
- FURY F4
- HGLRC AIO F3 v3 (SPRACINGF3) - works with build #739. Build #740 Arms but no response.  
- IMPULSERCF3 
- KISSFC
- LUX_RACE
- OMNIBUS
- OMNIBUSPRO (Blheli Configurator Hangs on read in 3.1, failed flash in 3.01)
- Omnibus F3 - build #737 works (is this OMNIBUSPRO or OMNIBUS?)
- RACEBASE FC
- REVO (and clones)
- SOULF4 - (SOULF4) build #734 (beeper not working) - works with REVO.hex
- SPracingF3 - Spektrum Sat now working. PPM now working since build #713.
- YuPi F4
- X-Racer V2.1 (SPRACINGF3 #670)
- X-Racer F303 (X_RACERSPI) -

####FC Targets that work with Dshot but require re-mapping pins with the Resource command:   
Boris' comment:  
  I had many requests for MOTOLAB (and other FCs) but that one doesn't have DMA available on all motors. It may be that we will assign one of the motors to PPM pin so you can resolder it. Not great, but better than nothing I guess?  

#####General instructions for re-mapping pins:   
 1- All FCs that require using the FC's PPM input pin as a motor output therefore can NOT use a PPM RX. Any of the Serial RXs that use a UART do work. Set-up Serial RX normally as needed for the FC board.  
 2- Check pins for the FC board below on which STM32 pins need to be re-mapped. It is a Good Idea to first type in the CLI:  
`resource  `  
`resource list  `  
and copy/paste these into a Text file and save for reference of the Default pin Mappings.    
 3- Solder a wire from the FC's header pin that will get re-mapped as a Dshot output to the motor output header pin per FC board detail below. Do not cut any traces, just short these two pins together which then allows switching pin mapping back to use BLHeli Pass-through without removing the FC from the copter.  NOTE: Some FCs re-map a different pin to use Dshot. See FC's below for details.  
  4- In the config tab select OneShot(42 or 125). Click Save. Leave this select until pins are re-mapped.  
  5- In the CLI type (x = motor #, yyy = STM32 pin #):   
`resource ppm none  `  
`resource motor x yyy  `  
`save  `  
  6- Now select the DSHOT protocol of your choice.  

#####To switch pin mapping to use BLHeli Suite pass-through:   
  1- Select OneShot(42 or 125) in the config tab. Click Save.   
  2- In the CLI change Only the remapped Motor back. Type:  
`resource motor x yyy  `  
`save  `  
Note: Re-mapping pin back to default may NOT be needed. Tested on MotoLab Cyclone and BLHeli works with motor 1 to A07.  
  3- Now use BLHeli Suite to Update or change ESC setting.  
  4- When finished re-map the motor to the pin used for Dshot.  
  5- Set ESC protocol to DSHOT.  

See: [CLI resource command](https://github.com/betaflight/betaflight/wiki/Betaflight-specific-CLI-commands#resource-remapping-command-v31)    

- MOTOLAB - (MotoF3, Cyclone & Tempest)   
Note: Tornado has output driver chips so can not add a wire to the output pin header. 
 3.1 Build #721: Bench tested -
 Solder a wire from Output #1 header pin to the PPM input header pin.
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`save  `  
To use BLHeli type in CLI:  Note: may not need to re-map pin.
`resource motor 1 A04  `  
`save  `  
 
- PIKOBLX - Re-map motor 1 to the PPM pin (same as MotoLab) and also disable motor 5-8 ("resource motor X none").    
 Solder a wire from Output #1 header pin to the PPM input header pin.
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`resource motor 5 none `  
`resource motor 6 none `  
`resource motor 7 none `  
`resource motor 8 none `  
`save  `  
 To use BLHeli type in CLI:  
`resource motor 1 A04  `  
`save  `  

- SPRACINGF3EVO -  Must move MOTOR 4 to new pin assignment (CLI = resource MOTOR 4 A06). Then solder ESC for motor #4 to motor output #5, fixes DMA conflict with motor outputs 2 and 4.  
 Solder a wire from Output #4 header pin to Output #5 header pin.
 Follow above and to re-map output type in CLI:  
`resource motor 5 none  `  
`resource motor 4 A06 `  
`save  `  
To use BLHeli type in CLI:  
`resource motor 4 A03  `  NEED the Default pin Number checked.  
`save  `  

- SPRACINGF3MINI - Solder a wire from motor 4 Output to the PPM pin. Then use resource command to disable PPM and map motor 4 output to B04.  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 4 B04 `  
`save  `  
To use BLHeli type in CLI:  
`resource motor 4 B09  `  
`save  `  
  Tested with 3.1 Build #783. 
Limitation: BlackBox on the internal SDCard works with MultiShot but not with DShot.  

- KOMBINI - Solder a wire from Output #1 header pin to the PPM input header pin.  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`save  `  
To use BLHeli type in CLI:  
`resource motor 1 A04  `  
`save  `   
Limitation: no LED functionality with DShot is allowed.  
Video on getting this FC working: https://www.rcgroups.com/forums/showpost.php?p=36269451&postcount=1966    

####FC Targets with DSHOT code added but in need of testing:
- SPARKY2
- Mantis F3 (from MRM) - SPRACINGF3  

###ESCs supporting Dshot:
- KISS 24 - 150, 300, 600 See the Dshot thread (link above) for ESC code.

####BLheli-S:
Firmware in Github here: https://github.com/bitdump/BLHeli    

Note: Many of these ESC have an RC filter on the control input signal line. The cap of this RC filter must be removed to use Dshot as per the list below. It is recommended to remove the cap from any ESC even if Dshot seems to work. All other protocols to still work without this filter cap.  

- Aikon 20A/Spedix 20A    
Cap removal for both: https://www.rcgroups.com/forums/showpost.php?p=36182572&postcount=1319
- Aikon SEFM v2 30A - 150, 300, 600 (BLHeli_S 16.43) (C-H-25)   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Aikon SEFM v1 30A - (without removing cap) 150, 300 (C-H-15)   Remove cap for 600-   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36213821&postcount=1611   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- AIKON SEFM 20A (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Armattan 20A and 30A - 20a v1 need cap removal, 30a need cap removal, 20a v2 will work without modification  
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36416316&postcount=2432
- BeeRotor BLS20A (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36217144&postcount=1654  
- BumpBee_S (with signal cap removed) - 600   
Cap removal: https://nathan.vertile.com/blog/2016/11/14/flash-blheli_s-with-dshot/#flashing 
- Cicada BB2 10A - 150,300 and 600 (no mods)
- Cicada v1 20A - 150
- Cicada v2 20A - 150, 300, 600 (BLHeli_S 16.43)
- Cicada v1 30A - 150 & 300
- Cicada v2 30A - 300 - 600 with cap removed 
- Cicada 4n1 20a V2 - 600 with caps removed  
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36286446&postcount=2036   
- Cicada 35Ax4 35A - 
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36384215&postcount=2337  
- DALRC BS25A  16.5 (with signal cap removed) - 150, 300, 600    
Cap removal: https://www.rcgroups.com/forums/showthread.php?t=2777858#post36223562   
https://www.rcgroups.com/forums/showthread.php?t=2777858
- DYS XS20 v1.x  
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36297135&postcount=2068
- DYS XS20A v2 or XSC20A
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36253268&postcount=1878
- DYS XS30A (with Signal cap removed) - 300 (16.42) - 600 (16.43)   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36133930&postcount=836   
https://www.rcgroups.com/forums/showpost.php?p=36245544&postcount=1848 
- DYS XSD20A | XSD30A - New product announcement:  
https://www.rcgroups.com/forums/showthread.php?t=2784634#post36278564  
- Emax Lightning S 35A - 600 without mods  
- FVT Littlebee 20A-S (with signal cap removed) - 150, 300, 600 
- LittleBee_S 20A  (A-H-15 16.42)- (with signal cap removed) - 150, 300, 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36181966&postcount=40934
- Littlebee_S 30A (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36230015&postcount=1745
- Lumenier 30A
- Maverick 24A - New product announcement:  
https://www.rcgroups.com/forums/showthread.php?2795245-Fully-compatible-with-Dshot-New-upgraded-32-bit-Maverick-24A-ESC#post36408093  
- MRM ZEUS-S 20A - with CAP removed (see FVT littlebee-s 20a cap removal)
- Multistar BLHeli_S 30A - 150, 300
- Racestat v2 6a (2-3S version) - 150, 300, 600 (BLHeli_S 16.43)
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36268041&postcount=1961
- Racestar v1 30A   
- Racerstar v2 30a - 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36209787&postcount=1569  
- Racestar v2 12A - 150, 300, 600 (BLHeli_S 16.43)
- Racestar v2 20A - 150, 300, 600 (BLHeli_S 16.43)
- Racerstar 25a - 300
- Racerstar v2 35A - 150, 300
- Racerstar MS25A - 150, 300, 600 (A_H_20 BL_S16.5) - report of motor twitching with 600. May need signal cap removed.
- Racerstar MS35A - 600
- Racerstar 4in1 20A V1 - 150 (one motor twitching), 300 (all running smooth), 600 (grinding noise) bench testing only so far   - This may need the signal cap(s) removed.  
-- 2016-12-10: Does this actually work for flying with dshot300 as well? Would be great to know.
Flying with dshot 300 but but clicking noises from motor. Sync problems?
- Racerstar RS20Ax4 V2 Blheli_S 20A 2-4S 4 in 1 ESC - 300 without mod.
Link: https://www.rcgroups.com/forums/showthread.php?t=2782732#post36254434
- Spedix ES 20 lite (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36203466&postcount=1509
- Spedix 25a (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36324412&postcount=2157   
- Spedix ES30 HV 16.43 (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36205800&postcount=1527  
- TBS 25 - 300 (600 with signal cap removed)   
Cap removal:   https://www.rcgroups.com/forums/showpost.php?p=36209076&postcount=1548
- Xrotor mini 30A BL_S - 300, Not working on 600    
- ZTW polaris 30A (A_H_20 16.42) - 300, 600

Generic instructions on how to find signal input and cap on any BLHeli ESC:
https://www.rcgroups.com/forums/showpost.php?p=36216745&postcount=1645   

#####Cap Removal Methods:  
*Caution - if improper done can render the ESC unusable and have the possibility to burning the ESC upon applying power. *  
- Method 1. Clippers - used this when there is enough room, just clipped the cap in half the clipped each end off the board  
- Method 2. Craft knife - used when other components are too close to use clippers, for example tbs escs - slide the blade so that it is sliding along the surface of the board and in a direction away from other components and apply force, cutting the cap off the esc   
- Method 3. Soldering iron - Heat both ends of the cap with a tinned iron. Then wipe the cap off the pads with the iron's tip. Be Careful NOT to unsolder any other parts and examine the board with a good magnifier for any solder splashes.  
- BeeRotor BS20A video- http://scontent.cdninstagram.com/t50.2886-16/15378420_376830535984172_2316137375807307776_n.mp4  

####Components tried and are not currently working:
NTR

####Components that will NOT likely ever work:
- Naze32 and clones
- All FCs with STM32F1
- All ESCs that can not run BLHeli_S firmware (except KISS24A)