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
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass-through. Also, some ESC that have a signal filter cap may not work until the cap is removed.   
Note: should be fixed on most Targets.
- BB logging  not working 
- 3D mode not yet working, developer's plan is to get the basics working well the get 3D working.
- A bug affecting Spektrum Sats, shows 988 on channels in receiver tab of Betaflight (soon to be looked at).
   Fixed on SPRF3, may be fixed on other targets.
- PPM not working on some FC targets (limited testing due to most users using Sbus or Spektrum)
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
Do type "get idle" to see if this exists and what the valid values are.

####ESC Cal and min/max throttle
ßF firmware with Dshot does Not use the min_throttle or max_throttle setting, these are ignored.
Just ensure that in the ESCs (BLHeli Suite) that PPM_MIN_THROTTLE is set to 1000 and PPM_MAX_THROTTLE is set to 2000.

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




####Flight Controllers Supporting Dshot on Betaflight 3.1 Alphas:
- BLUEJAYF4
- REVO (and clones)
- OMNIBUS
- OMNIBUSPRO (Blheli Configurator Hangs on read in 3.1, failed flash in 3.01)
- KISSFC
- LUX_RACE
- BETAFLIGHTF3
- FLIP32F4
- FURY F4
- Airbot F3 (spracingf3)
- Dodo (spracingf3)
- Dodo (RMDO)
- X-Racer V2.1 (spracingf3 #670)
- DTFC (DOGE) (build 682)
- SPracingF3 - Spektrum Sat now working. PPM now working since build #713.
- IMPULSERCF3 
- DTFc - Build #389 - working on the bench
- RACEBASE FC
- YuPi F4

####Intermittent FC Reports:
- KOMBINI - reported not to connect to config.

####FC Targets with DSHOT code added but in need of testing:

- MOTOLAB - Locked out Comm port when Dshot150 enabled. Short Boot pins to re-flash (build #683).
Boris' comment:  
I had many requests for MOTOLAB but that one doesn't have DMA available on all motors. It may be that we will assign one of the motors to PPM pin so you can resolder it. Not great, but better than nothing I guess?
- SPARKY2
- SPRACINGF3EVO - Locked out Comm port fixed (build 708?). Must move MOTOR 4 to new pin assignment (CLI = resource MOTOR 4 A06). Then solde ESC for motor #4 to motor output #5, fixes DMA conflict with motor outputs 2 and 4. See: https://github.com/betaflight/betaflight/wiki/Betaflight-specific-CLI-commands#resource-remapping-command-v31
- FURYF3
- SOULF4
- RACEBASE

####ESCs supporting Dshot:
- KISS 24 - 150, 300, 600 See the Dshot thread (link above) for ESC code.

####BLheli-S:
Firmware in Github here: https://github.com/bitdump/BLHeli    

- Cicada v1 20A - 150
- Cicada v1 30A - 150 & 300
- Racestar v1 30A   
- Racerstar v2 30a - 600 
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36209787&postcount=1569  
- Racestar v2 20A & Cicada 20A - 150, 300, 600 (BLHeli_S 16.43)
- Racestar v2 12A - 150, 300, 600 (BLHeli_S 16.43)
- Racerstar 25a - 300
- Racerstar v2 35A - 150, 300
- Racerstar MS35A - 600
- Racerstar 4in1 20A V1 - 150 (one motor twitching), 300 (all running smooth), 600 (grinding noise) bench testing only so far   - This may need the signal cap(s) removed.  
- Aikon 20A/Spedix 20A 
Cap removal for both: https://www.rcgroups.com/forums/showpost.php?p=36182572&postcount=1319
- Aikon SEFM v2 30A - 150, 300, 600 (BLHeli_S 16.43) (C-H-25)
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Aikon SEFM v1 30A - 150, 300 (C-H-15)
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36213821&postcount=1611
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- AIKON SEFM 20A (with signal cap removed) - 150, 300, 600
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36214609&postcount=1614
- Lumenier 30A
- TBS 25 - 300 (600 with signal cap removed)   
Cap removal:   https://www.rcgroups.com/forums/showpost.php?p=36209076&postcount=1548
- DYS XS30A (with Signal cap removed) - 300 (16.42)  
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36133930&postcount=836
- ZTW polaris 30A (A_H_20 16.42) - 300, 600
- Multistar BLHeli_S 30A - 150, 300
- FVT Littlebee 20A-S (with signal cap removed) - 150, 300, 600
- LittleBee_S 20A  (A-H-15 16.42)- (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36181966&postcount=40934
- Spedix ES 20 lite (with signal cap removed) - 150, 300, 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36203466&postcount=1509
- Spedix ES30 HV 16.43 (with signal cap removed) - 600   
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36205800&postcount=1527  
- BumpBee_S (with signal cap removed) - 600   
Cap removal: https://nathan.vertile.com/blog/2016/11/14/flash-blheli_s-with-dshot/#flashing  
- BeeRotor BLS20A (with signal cap removed) - 600
Cap removal: https://www.rcgroups.com/forums/showpost.php?p=36217144&postcount=1654   

Generic instructions on how to find signal input and cap on any BLHeli ESC:
https://www.rcgroups.com/forums/showpost.php?p=36216745&postcount=1645   

####Combos (FC & ESC) working:

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
- BlueJayF4 & DYS XS30 (Cap off) - 300 (was working on build 667, may need rework on 689)
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
- Dodo with RMDO hex & Racerstar RS12A V2 (G_H_30) - 300
- X-Racer f303 v3 & Racerstar v2 35a (2s-6s) - 150, 300
- DTFC (DOGE) (build 682) & Cicada v2 20A (G_H_30) - 300
- DTFC (DOGE) & Littlebee 20A-S with cap removed (A_H_15) - 300, 600
- IMPULSERCF3 & ZTW polaris 30A
- Fury F4 & Cicada V1 - 150 & 300
- YuPi F4 & Aikon SEFM 30

####Components tried and are not currently working:

- DYS XS20
- DYS XS30 with signal cap left on
- AIKON v2 20A
- DALRC BS25A (The featured DSHOT BS25A ESC will be available on the later production , for current version in order 
  to support Dshot you have to modeify the filter circuit of the input signal. https://www.rcgroups.com/forums/showthread.php?t=2777858)
- Littlebee_S 30A
- Spedix 25a blheli_s

####Components that will NOT likely ever work:
- Naze32 and clones
- All FCs with STM32F1
- All ESCs that can not run BLHeli_S firmware (except KISS24A)