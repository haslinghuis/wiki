# Dshot & BetaFlight 3.1   
Dshot is new and currently code is being developed and tested. Since there are many dependencies and not all current hardware works this is to help keep track.

The Official Thread for Dshot is: https://www.rcgroups.com/forums/showthread.php?t=2756129   
Please Read this thread for details of how it works and up to date info. The First Post of this thread has all the needed Links to code and other info. A good article on Dshot and ESC protocols is here:   
https://blck.mn/2016/11/dshot-the-new-kid-on-the-block/  

ESC Protocol videos by Joshua Barwell:  
https://www.rcgroups.com/forums/showpost.php?p=36552970&postcount=2765  

In the Dshot thread is this post on how to re-Flash ESCs & FC:   
https://www.rcgroups.com/forums/showpost.php?p=36200950&postcount=1468   

For KISS FC's and ESCs see the KISS Dshot thread:  
https://www.rcgroups.com/forums/showthread.php?t=2780055

[BetaFlight V3.1 RC5 released](/betaflight/betaflight/wiki/BetaFlight-V3.1.x)  
Just remember that this is still Experimental code and may have serious limitations.  
Read here to Learn exactly what firmware issues and features are being worked on:  
https://github.com/betaflight/betaflight/issues?q=is%3Aissue+is%3Aclosed+sort%3Aupdated-desc  

The Latest Development version of the Configurator is recommended. Link on [Home page](https://github.com/betaflight/betaflight/wiki).  

Some known limitations are (Note: All should be fixed in the 3.1 release but leaving this list until full testing has been done by users):  
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass-through.    
Note: Should be fixed on most Targets.   
- New pass-through for KISS ESCs: [ESC Pass-through](https://github.com/betaflight/betaflight/wiki/Betaflight-ESC-pass-through)  
- Some ESCs that have a signal filter cap may not work until the cap is removed. 
- Dshot for 3D is implemented in the latest betaflight master. I am pretty sure that betaflight code is now correct, but I haven't fully tested all escs ans didn't do any real flight testing.  
3D implementation in DSHOT is slightly different than on analog protocols. Boris.  
I believe KISS escs now have that implementation. I am not sure of the latest blheli_s status.
- A bug affecting Spektrum Sats, shows 988 on channels in receiver tab of Betaflight (soon to be looked at).
   Fixed on SPRF3 & REVO, may be fixed on other targets.  
- PPM not working on some FC targets (limited testing due to most users using Sbus or Spektrum)   
   Note: Some targets need motor re-mapped to PPM pin so then PPM can not be used.
- LEDs not working or must be disabled to have all motors working due the DMA mapping conflicts.  
- The 'target.c' source files show that only Quad copters are supported in most targets and some targets support Hex copters. ?? Anyone trying a Hex?
- Some targets do not work if a Custom mixer is enabled. Instead use the new "resource" CLI command instead. Just be aware that not all outputs (pins) can be assigned a DMA channel for Dshot.  

####A quick way to determine IF the hex flashed supports Dshot:  
Go to the CLI and type "get pwm". All settings with 'pwm' in the name will be shown with all options.
If DSHOT150, DSHOT300, DSHOT600 is NOT in the list for the "motor_pwm_protocol" then this firmware does NOT support Dshot.
Example for NAZE that does Not support Dshot: 

motor_pwm_protocol = ONESHOT42  
Allowed values: OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED

####Setting Min Throttle with Dshot   
Dshot now uses the CLI command "digital_idle_percent" which adds to the 'min_throttle'.
Do type "get idle" to see if this exists and what the valid values are. This is a percentage of throttle used in armed state. The pid controller will never push use motor output below this percentage. Test it out by arming while watching the motors tab. If the motors idle slower when using dshot, bump up the digital idle percent. I set mine to 4.000 to make my motors arm at the same speed when swapping back and forth between multi and dshot. 

The configurator will show the idle speed in percent also for PWM protocols.

####ESC Cal and min/max throttle
ßF firmware with Dshot does Not use the min_throttle or max_throttle setting, these are ignored.
Just ensure that in the ESCs (BLHeli Suite) that PPM_MIN_THROTTLE is set to 1000 and PPM_MAX_THROTTLE is set to 2000.
Note: This should not be needed in BLHeli_S 16.43 and up since the PPM_MIN & MAX values are not used for Dshot.   

####Max ESC update speed supported by different dshot versions:  
#####WARNING: due to processor tasks, FC and/or ESC, the maximum update rate may not work-  
#####TEST without props and a Current Limiter.  

Theoretical speeds are a lot higher. The speed is limited in firmware to give more spreading between signals.
- DSHOT150: 4kHz max
- DSHOT300: 10,6kHz max (10,6khz is only available on 32khz gyro boards)
- DSHOT600: 16kHz max
- DSHOT1200: >32khz max (Currently only KISS24 supports DSHOT1200)

Note: When DSHOT is enabled Unsyced PWM is disabled. DSHOT always runs at the PID loop rate.  

####Dshot digital Values:  
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
[DSHOT O'scope captures](https://www.rcgroups.com/forums/showpost.php?p=36490579&postcount=2631) before and after the typical RC filter.  
BLHeli Rev16.5 posted on github (27Nov2016). Dshot150, Dshot300 and Dshot600 are now supported officially. [link](https://github.com/bitdump/BLHeli)   

###Flight Controllers Tested to Support Dshot on Betaflight 3.1 Alphas:
- AIORACERF3
- Airbot F3 (SPRACINGF3)  
- BETAFLIGHTF3  
- BLUEJAYF4
- BrainFPV RE1 (needs build from BrainFPV repo)
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
Note from sskaug: KISS FCs use a resistor to drive the throttle signal high (pull-up). So it is quite sensitive to capacitance on the ESC signal.  
- LUX_RACE
- LUXV2_RACE
- OMNIBUS
- OMNIBUSPRO (Blheli Configurator Hangs on read in 3.1, failed flash in 3.01)
- Omnibus F3 - build #737 works (is this OMNIBUSPRO or OMNIBUS?)
- RACEBASE FC
- REVO (and clones)
- Rotorgeeks RG SSD -Currently only a custom build. Will be added to official builds soon.
- SIRINFPV
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

#####To switch pin mapping to use BLHeli Suite pass-through:   May not be required.  
  1- Select OneShot(42 or 125) in the config tab. Click Save.   
  2- In the CLI change Only the remapped Motor back. Type:  
`resource motor x yyy  `  
`save  `  
######Note: Re-mapping pin back to default may NOT be needed. 
  3- Now use BLHeli Suite to Update or change ESC setting.  
  4- When finished re-map the motor to the pin used for Dshot.  
  5- Set ESC protocol to DSHOT.  

See: [CLI resource command](https://github.com/betaflight/betaflight/wiki/Betaflight-resource-remapping)    

- MOTOLAB - (MotoF3, Cyclone & Tempest)   
 Solder a wire from Output #1 header pin to the PPM input header pin. [Photo of wire on a Cyclone](https://www.rcgroups.com/forums/showpost.php?p=36589146&postcount=2787)   
Note: Adding this wire is not required if you connect signal wire from ESC #1 directly to the PPM pin.  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`save  `  
No remapping back to default or selecting a non-Dshot protocol is required to use BLHeli pass-through(tested with 3.1RC4).
 
- PIKOBLX - Re-map motor 1 to the PPM pin (same as MotoLab) and also disable motor 5-8 ("resource motor X none").    
 Solder a wire from Output #1 header pin to the PPM input header pin.  
Link to modification details: https://www.rcgroups.com/forums/showpost.php?p=36608148&postcount=43149  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`resource motor 5 none `  
`resource motor 6 none `  
`resource motor 7 none `  
`resource motor 8 none `  
`save  `  
 To use BLHeli type in CLI:  May not be required.  
`resource motor 1 A04  `  
`save  `  


- SPRACINGF3EVO -  Must move MOTOR 4 to new pin assignment (CLI = resource MOTOR 4 A06). Then solder ESC for motor #4 to motor output #5, fixes DMA conflict with motor outputs 2 and 4.  
 Solder a wire from Output #4 header pin to Output #5 header pin.
 Follow above and to re-map output type in CLI:  
`resource motor 5 none  `  
`resource motor 4 A06 `  
`save  `  
To use BLHeli type in CLI:    May not be required.  
`resource motor 4 A03  `  NEED the Default pin Number checked.  
`save  `  

- SPRACINGF3MINI - Solder a wire from motor 4 Output to the PPM pin. Then use resource command to disable PPM and map motor 4 output to B04.  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 4 B04 `  
`save  `  
To use BLHeli type in CLI:    May not be required.  
`resource motor 4 B09  `  
`save  `  
  Tested with 3.1 Build #783. 
Limitation: BlackBox on the internal SDCard works with MultiShot but not with DShot.  

- KOMBINI - Solder a wire from Output #1 header pin to the PPM input header pin.  
 Follow above and to re-map output type in CLI:  
`resource ppm none  `  
`resource motor 1 A07 `  
`save  `  
To use BLHeli type in CLI: May not be required.  
`resource motor 1 A04  `  
`save  `   
Limitation: no LED functionality with DShot is allowed.  
Video on getting this FC working: https://www.rcgroups.com/forums/showpost.php?p=36269451&postcount=1966    

- AIORACERF3 -  Must move MOTOR 2 to new pin assignment since it has a DMA conflict with MOTOR 1:    
Connect ESC for motor 2 to 'LED' pin on the board.  
 Then re-map motor 2 output: type in CLI:    
 `resource motor 2 A08`  
 `save`  
 The new mapping for motor 2 conflicts wit `LED_STRIP` and `TRANSPONDER`, so make sure these two features are disabled in the configurator.   
To use BLHeli type in CLI:  May not be required.      
`resource motor 4 A07`   
`save`  

- SPRacing F3 NEO - motor 2 on pin C07 not assigned a DMA channel when Dshot is selected.
Need a work around for this FC.  Probably same as other FCs by re-mapping Motor 2 to the PPM pin (A03).  
Anyone with this FC can try (this is NOT TESTED and needs to be tried and reported):
Move motor2's ESC signal to the PPM FC pin.
type in CLI:  
`resource ppm none  `  
`resource motor 2 A03 `  
One report that this Does NOT work since the Serial RX is also on the PPM pin.  
Need to try another fix.  

####FC Targets with DSHOT code added but in need of testing:
- SPARKY2
- Mantis F3 (from MRM) - SPRACINGF3  
- All other targets not listed above.   

###ESCs supporting Dshot:
- KISS 24 - 150, 300, 600, 900, 1200 See the Dshot thread (link above) for ESC code.

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
- Armattan 20A and 30A - 2v1 need cap removal, v2 (now shipping from Armattan) work without modification  
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
- Cicada 35Ax4 35A - 600 with caps removed   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36384215&postcount=2337  
- Cobra CP30A ESC 30A BLheli_S - 600  
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
- FVT LittleBee-Spring 20Ax4 - Claims Dshot but Unknown  
Thread: https://www.rcgroups.com/forums/showthread.php?2800280-Favourite-FVT-LittleBee-Spring-20Ax4-4-in-1-Blheli_S-ESC-w-built-in-5V-12V-BEC  
- LittleBee_S 20A  (A-H-15 16.42)- (with signal cap removed) - 150, 300, 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36181966&postcount=40934
- Littlebee_S 30A (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36230015&postcount=1745
- Lumenier 30A
- Maverick 24A - New product announcement:  
https://www.rcgroups.com/forums/showthread.php?2795245-Fully-compatible-with-Dshot-New-upgraded-32-bit-Maverick-24A-ESC#post36408093  
- MRM ZEUS-S 20A - with CAP removed (see FVT littlebee-s 20a cap removal)
- Multistar BLHeli_S 30A - 150, 300
- Racestar v2 6a (2-3S version) - 150, 300, 600 (BLHeli_S 16.43)
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36268041&postcount=1961
- Racerstar v1 20A - 150, 300 (worked with Lux FC, no cap. mod)
- Racestar v1 30A   
- Racerstar v2 30a - 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36209787&postcount=1569  
- Racestar v2 12A - 150, 300, 600 (BLHeli_S 16.43)
- Racestar v2 20A - 150, 300, 600 (BLHeli_S 16.43)
- Racerstar 25a - 300
- Racerstar v2 35A - 150, 300
- Racerstar MS25A - 150, 300, 600 (A_H_20 BL_S16.5) - report of motor twitching with 600. May need signal cap removed.
- Racerstar MS35A - 600  
Cap removal with before and after O'scope traces: https://www.rcgroups.com/forums/showpost.php?p=36588994&postcount=2834  
- Racerstar 4in1 20A V1 - 150 (one motor twitching), 300 (all running smooth), 600 (grinding noise) bench testing only so far   - This may need the signal cap(s) removed.   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36550495&postcount=178  
- Racerstar RS20Ax4 V2 Blheli_S 20A 2-4S 4 in 1 ESC - 300 without mod.  
Link: https://www.rcgroups.com/forums/showthread.php?t=2782732#post36254434
- Racestar RS20A v2 - 150, 300 - with cap removed 600. Also reported works at 600 without the cap removed from a  Xracer 303 V3 FC.  
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36503849&postcount=2672  
- Racerstar RS30A LiteS - 600 with cap removed.  
Cap removal: https://www.rcgroups.com/forums/showthread.php?2690769-Racerstar-Blheli_S-ESCs-%28v2-just-released%29-10-for-20a-12-for-30a-30-4in1-20a/page38  
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