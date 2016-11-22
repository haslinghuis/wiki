# Dshot & BetaFlight   
Dshot is new and currently code is being developed and tested. Since there are many dependencies and not all current hardware works this is to help keep track.

The Official Thread for Dshot is: https://www.rcgroups.com/forums/showthread.php?t=2756129
Please Read this thread for details of how it works and up to date info. The First Post of this thread has all the needed Links to code and other info. A good article on Dshot and ESC protocols is here: https://blck.mn/2016/11/dshot-the-new-kid-on-the-block/  
In the Dshot thread is this post on how to re-Flash ESCs & FC: https://www.rcgroups.com/forums/showpost.php?p=36200950&postcount=1468

BetaFlight development code for Dshot can be found here:
http://andwho.sytes.net:8080/job/BorisB_BetaFlight/
Just remember that this is very Experimental code and may have serious limitations.

Some known limitations are (
Note: these may or may not affect all working targets):
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass-through.    
Note: should be fixed on most Targets but not all targets yet.   
- Some ESCs that have a signal filter cap may not work until the cap is removed. 
- BB logging  not working. Seems to work on many targets but unable to download to a file, erase worked.
- 3D mode not yet working, developer's plan is to get the basics working well the get 3D working.
- A bug affecting Spektrum Sats, shows 988 on channels in receiver tab of Betaflight (soon to be looked at).
   Fixed on SPRF3 & REVO, may be fixed on other targets.
- PPM not working on some FC targets (limited testing due to most users using Sbus or Spektrum)   
   Note: Some targets need motor re-mapped to PPM pin so then PPM can not be used.
- LEDs not working  
- The 'target.c' source files show that only Quad copters are supported in most targets and some targets support Hex copters.
- Some targets do not work if a Custom mixer is enabled. Instead use the new "resource" CLI command instead. Just be aware that not all outputs (pins) can be assigned a DMA channel for Dshot.  

####A quick way to determine IF the hex flashed supports Dshot:  
Go to the CLI and type "get pwm". All settings with 'pwm' in the name will be shown with all options.
If DSHOT150, DSHOT300, DSHOT600 is NOT in the list for the "motor_pwm_protocol" then this firmware does NOT support Dshot.
Example from built 668 for MotoLab that does Not support Dshot: 

motor_pwm_protocol = ONESHOT42
Allowed values: OFF, ONESHOT125, ONESHOT42, MULTISHOT, BRUSHED

####Setting Min Throttle with Dshot   
Dshot now uses the CLI command " digital_idle_offset" and the old 'min_throttle' setting is ignored.
Do type "get idle" to see if this exists and what the valid values are. To set this correctly this value needs to be double whatever the difference is between min command (which should be 1000) and whatever value your min throttle  was set at when using multishot / oneshot. For example if your min throttle when using multishot was 1040 (difference here between min command and min throttle is 40) then you should set your digital idle offset to 80. Or if your min throttle pre dshot was 1055 (difference between min command and min throttle is here is 55) then you set digital idle offset to 110. You can play with these values a little but if you experience motor/prop stalls during flight this number should be raised as having this value too low could be the cause. 

####ESC Cal and min/max throttle
ßF firmware with Dshot does Not use the min_throttle or max_throttle setting, these are ignored.
Just ensure that in the ESCs (BLHeli Suite) that PPM_MIN_THROTTLE is set to 1000 and PPM_MAX_THROTTLE is set to 2000.
Note: This should not be needed in BLHeli 16.43 since the PPM_MIN & MAX values are not used for Dshot.   

####Max ESC update speed supported by different dshot versions:
- dshot150: 8khz max
- dshot300: 16khz max
- dshot600: 32khz max

Note: When DSHOT is enabled Unsyced PWM is disabled. DHSOT always runs at the PID loop rate.

###Starting with a post from the Dshot thread:

Scouring the BF and Dshot threads has resulted in the following lists. The information posted herein is all provided on an AS-IS basis.
It has been derived from posts on this forum (RCGroups/BEtaflight & DSHOT threads), and direct correspondences with users.

USE THE FOLLOWING INFORMATION AT YOUR OWN RISK

BETAFLIGHT AND DSHOT -

Betaflight may not be able to support all Targets ( or specific boards) due to the design of each one differing on the DMA pinout. Look below to find what may or may not be working.

DSHOT on BLHELI-S ESCs will likely only support Dshot150 & 300.
Here is why: https://www.rcgroups.com/forums/showpost.php?p=36025232&postcount=376   
Now found that some BLHeli_S ESC will run on DSHOT600 with modifications. See list below for details.

####Flight Controllers Supporting Dshot on Betaflight 3.1 Alphas:
- BLUEJAYF4
- REVO (and clones)
- OMNIBUS
- OMNIBUSPRO (Blheli Configurator Hangs on read in 3.1, failed flash in 3.01)
- Omnibus F3 - build #737 works (is this OMNIBUSPRO or OMNIBUS?)
- KISSFC
- LUX_RACE
- BETAFLIGHTF3
- FLIP32F4
- FURY F4
- Airbot F3 (spracingf3)
- Dodo (spracingf3)
- Dodo (RMDO)
- X-Racer V2.1 (spracingf3 #670)
- DTFc - Build #389 - working on the bench
- DTFC (DOGE) (build 682)
- SPracingF3 - Spektrum Sat now working. PPM now working since build #713.
- IMPULSERCF3 
- RACEBASE FC
- YuPi F4
- Colibri Race v2.0  (build #722)
- SOULF4 - works with SOULF4.hex, build #734 (beeper not working) - works with REVO.hex

####Intermittent FC Reports:
- KOMBINI - reported not to connect to config (still on #721).  This FC needs to re-map the motor pin the same way as for the MotoLab FC below. Reports are that this FC has the exact same pin out as the MotoLab FCs with the exception the Sbus in UART3.

####FC Targets with DSHOT code added but in need of testing:
- FURYF3
- RACEBASE
- SPARKY2
- Seriously Pro Mini - locks up when Dshot selected.
- MOTOLAB - (MotoF3, Tornado, Cyclone & Tempest) Beeper not working    
Boris' comment:  
I had many requests for MOTOLAB but that one doesn't have DMA available on all motors. It may be that we will assign one of the motors to PPM pin so you can resolder it. Not great, but better than nothing I guess?   
The new Betaflight 3.1 code has the "resource" CLI command . To use Dshot, you'll need move the motor 1 signal to the PPM pin.  

3.1 Build #721: Bench tested -     Do the following to setup for Dshot.

`In Port Tab Set UART2 to SerialRX- save              ; Must use a serial port    `   
`In Config Tab Set RX to Serial (SBUS, etc)- save   ; Do NOT set ESC to DSHOT yet, leave as OneShot125    `   
`In CLI type:                                           `   
`resource ppm none                                  ; Disables use of PPM    `   
`resource motor 1 A07                               ; Assigns motor 1 to the PPM Pin    `   
`save                                               ; reboots    `   
`In Config Tab set to desired DSHOT protocol- save    `    
 
- SPRACINGF3EVO - Locked out Comm port fixed (build 708?). Must move MOTOR 4 to new pin assignment (CLI = resource MOTOR 4 A06). Then solder ESC for motor #4 to motor output #5, fixes DMA conflict with motor outputs 2 and 4. 

See: [CLI resource command](https://github.com/betaflight/betaflight/wiki/Betaflight-specific-CLI-commands#resource-remapping-command-v31)

####ESCs supporting Dshot:
- KISS 24 - 150, 300, 600 See the Dshot thread (link above) for ESC code.

####BLheli-S:
Firmware in Github here: https://github.com/bitdump/BLHeli    

- Cicada BB2 10A - 150,300 and 600 (no mods)
- Cicada v1 20A - 150
- Cicada v2 20A - 150, 300, 600 (BLHeli_S 16.43)
- Cicada v1 30A - 150 & 300
- Cicada v2 30A - 300
- Racestar v1 30A   
- Racerstar v2 30a - 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36209787&postcount=1569  
- Racestar v2 12A - 150, 300, 600 (BLHeli_S 16.43)
- Racestar v2 20A - 150, 300, 600 (BLHeli_S 16.43)
- Racerstar 25a - 300
- Racerstar v2 35A - 150, 300
- Racerstar MS35A - 600
- Racerstar 4in1 20A V1 - 150 (one motor twitching), 300 (all running smooth), 600 (grinding noise) bench testing only so far   - This may need the signal cap(s) removed.  
- Racerstar RS20Ax4 V2 Blheli_S 20A 2-4S 4 in 1 ESC - 300 without mod.
Link: https://www.rcgroups.com/forums/showthread.php?t=2782732#post36254434
- Aikon 20A/Spedix 20A    
Cap removal for both: https://www.rcgroups.com/forums/showpost.php?p=36182572&postcount=1319
- Aikon SEFM v2 30A - 150, 300, 600 (BLHeli_S 16.43) (C-H-25)   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Aikon SEFM v1 30A - (without removing cap) 150, 300 (C-H-15)   Remove cap for 600-   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36213821&postcount=1611   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- AIKON SEFM 20A (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Lumenier 30A
- TBS 25 - 300 (600 with signal cap removed)   
Cap removal:   https://www.rcgroups.com/forums/showpost.php?p=36209076&postcount=1548
- DYS XS20A or XSC20A 
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36253268&postcount=1878
- DYS XS30A (with Signal cap removed) - 300 (16.42) - 600 (16.43)   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36133930&postcount=836   
https://www.rcgroups.com/forums/showpost.php?p=36245544&postcount=1848   
- ZTW polaris 30A (A_H_20 16.42) - 300, 600
- Multistar BLHeli_S 30A - 150, 300
- FVT Littlebee 20A-S (with signal cap removed) - 150, 300, 600   
- LittleBee_S 20A  (A-H-15 16.42)- (with signal cap removed) - 150, 300, 600    
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36181966&postcount=40934
- Littlebee_S 30A (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36230015&postcount=1745
- Spedix ES 20 lite (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36203466&postcount=1509
- Spedix 25a (with signal cap removed) - 150, 300, 600   
- Spedix ES30 HV 16.43 (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36205800&postcount=1527  
- BumpBee_S (with signal cap removed) - 600   
Cap removal: https://nathan.vertile.com/blog/2016/11/14/flash-blheli_s-with-dshot/#flashing  
- BeeRotor BLS20A (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36217144&postcount=1654   
- Xrotor mini 30A BL_S - 300, Not working on 600    
- DALRC BS25A (with signal cap removed) - 150, 300??    
Cap removal: https://www.rcgroups.com/forums/showthread.php?t=2777858#post36223562   
https://www.rcgroups.com/forums/showthread.php?t=2777858

Generic instructions on how to find signal input and cap on any BLHeli ESC:
https://www.rcgroups.com/forums/showpost.php?p=36216745&postcount=1645   

####Combos (FC & ESC) working:   
These should be removed since If an ESC works and an FC works then together they should work. Therefore no need to list all these combinations.   

- KISSFC & KISS 24 - 150, 300, 600
- KISSFC & Racesta/CIcada 20A - 150
- KISSFC & TBS25 - 300
- OMNIBUS (with blackbox SD feature off) & KISS24
- OMNIBUSF4 & KISS24
- OMNIBUSF4 & AIKON SEFM 30A (V2 C-H-25 using DS600)(BLheli_S 16.43)
- REVO & KISS24
- REVO & AIKON 30A (v1 (150, 300, 600) & v2)
- REVO + Racestar v2 20a (G_H_30) - 300
- REVO + Racestar v2 20a (G_H_30) - 300, 600
- BLUEJAYF4 & KISS24
- BlueJayF4 & Aikon SEFM 30
- BlueJayF4 & v2 Racestar/Cicada 20a
- BlueJayF4 & v2 Racestar/Cicada 30a (1 report of BJF4 and Racestar v2 30a not working)
- BlueJayF4 & DYS XS30 (Cap off) - 300, 600 (build #723)
- Xracer 3.1 SPI & v2 Racestar/Cicada 20a - 150, 300
- FLIP32F4 & Racerstar25A - 150, 300
- LUXFC & Luminier 30A - 300
- LUXFC & v2 Racestar/Cicada 30A - 150
- FURY F4 & KISS24 - 600
- FURY F4 & v2 Racestar/Cicada 20a - 150, 300, 600 (BLHeli_S 16.43)
- AIRBOT F3 & Cicada 20A - 150, 300 (Airbot F3 uses SPRACING F3 target)
- SPRACINGF3 on build 669 & ZTW polaris (A_H_20 16.42) - 300
- SPRACINGF3 on build 713 & Racerstar MS35A (A_H_20 16.43) - 600
- Dodo with SPRACINGF3 hex & Cicada v1 20A (G_L_30) - 150
- Dodo with SPRACINGF3 hex (#721) &  Kiss ESCs with Dshot600
- Dodo with RMDO hex & Racerstar RS12A V2 (G_H_30) - 300
- X-Racer f303 v3 & Racerstar v2 35a (2s-6s) - 150, 300
- DTFC (DOGE) (build 682) & Cicada v2 20A (G_H_30) - 300
- DTFC (DOGE) & Littlebee 20A-S with cap removed (A_H_15) - 300, 600
- IMPULSERCF3 & ZTW polaris 30A
- Fury F4 & Cicada V1 - 150 & 300
- YuPi F4 & Aikon SEFM 30
- MotoLab cyclone (build 730) & Kiss24A - 600

####Components tried and are not currently working:
- DYS XS20
- DYS XS30 with signal cap left on
- AIKON v2 20A - ?? is this the same ESC as listed as working above ??

####Components that will NOT likely ever work:
- Naze32 and clones
- All FCs with STM32F1
- All ESCs that can not run BLHeli_S firmware (except KISS24A)