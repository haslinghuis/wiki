# Dshot & BetaFlight   
Dshot is new and currently code is being developed and tested. Since there are many dependencies and not all current hardware works this is to help keep track.

The Official Thread for Dshot is: https://www.rcgroups.com/forums/showthread.php?t=2756129
Please Read this thread for details of how it works and up to date info. The First Post of this thread has all the needed Links to code and other info. A good article on Dshot and ESC protocols is here: https://blck.mn/2016/11/dshot-the-new-kid-on-the-block/  

BetaFlight development code for Dshot can be found here:
http://andwho.sytes.net:8080/job/BorisB_BetaFlight/
Just remember that this is very Experimental code and may have serious limitations.

Some known limitations are (
Note: these may or may not affect all working targets):
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass-through.
- BB logging  not working 
- 3D mode not yet working, developer's plan is to get the basics working well the get 3D working.
- A bug affecting Spektrum Sats, shows 988 on channels in receiver tab of Betaflight (soon to be looked at).
   Fixed on SPRF3, may be fixed on other targets.
- PPM not working on some FC targets (limited testing due to most users using Sbus or Spektrum)
- LEDs not working  
- The 'target.c' source files show that only Quad copters are supported in most targets and some targets support Hex copters.

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
- OMNIBUSPRO
- KISSFC
- LUX_RACE
- BETAFLIGHTF3
- FLIP32F4
- FURY F4
- Airbot F3 (spracingf3)
- Dodo (spracingf3)
- X-Racer V2.1 (spracingf3 #670)
- DTFC (DOGE) (build 682)
- SPracingF3 - Spektrum Sat now working. PPM not working.
- IMPULSERCF3 
- DTFc - Build #389 - working on the bench

####Intermittent FC Reports:
- KOMBINI - reported not to connect to config.

####FC Targets with DSHOT code added but in need of testing:

- MOTOLAB - Locked out Comm port when Dshot150 enabled. Short Boot pins to re-flash (build #683).
Boris' comment:  
I had many requests for MOTOLAB but that one doesn't have DMA available on all motors. It may be that we will assign one of the motors to PPM pin so you can resolder it. Not great, but better than nothing I guess?
- SPARKY2
- SPRACINGF3EVO - Locked out Comm port fixed (build 708?). Must move MOTOR 4 to new pin assignment (CLI = resource MOTOR 4 A06). Then solde ESC for motor #4 to motor output #5, fixes DMA conflict with motor outputs 2 and 4. See: https://github.com/betaflight/betaflight/wiki/Betaflight-specific-CLI-commands#resource-remapping-command-v31
- RMDO
- FURYF3
- SOULF4

####ESCs supporting Dshot:
- KISS 24 - 150, 300, 600 See the Dshot thread (link above) for ESC code.

####BLheli-S:
Firmware in Github here: https://github.com/bitdump/BLHeli

- Cicada v1 20A - 150
- Racestar v1 30A
- Racestar v2 20A & Cicada 20A - 150, 300, 600? (BLHeli_S 16.43)
- Racerstar 25a - 300
- Racerstar v2 35A - 150, 300
- Aikon SEMF v1 30A - 150, 300, 600 (BLHeli_S 16.43)
- Aikon SEMF v2 30A - 150, 300
- Lumenier 30A
- TBS 25 - 300
- DYS XS30A (with Signal cap removed) - 300 (16.42) https://www.rcgroups.com/forums/showpost.php?p=36133930&postcount=836
- ZTW polaris 30A (A_H_20 16.42) - 300, 600
- Multistar BLHeli_S 30A - 150, 300
- FVT Littlebee 20A-S (with cap removed) - 150, 300, 600
- LittleBee_S 20A  (A-H-15 16.42)- (with Cap removed) - 150, 300, 600   
https://www.rcgroups.com/forums/showpost.php?p=36181966&postcount=40934

####Combos (FC & ESC) working:

- KISSFC & KISS 24 - 150, 300, 600
- KISSFC & Racesta/CIcada 20A - 150
- KISSFC & TBS25 - 300
- OMNIBUS (with blackbox SD feature off) & KISS24
- OMNIBUSF4 & KISS24
- REVO & KISS24
- REVO & AIKON 30A (v1 (150, 300, 600) & v2)
- REVO + Racestar v2 20a (G_H_30) - 300
- BLUEJAYF4 & KISS24
- BlueJayF4 & Aikon Semf 30
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
- Dodo with SPRACINGF3 hex & Cicada v1 20A (G_L_30) - 150
- X-Racer f303 v3 & Racerstar v2 35a (2s-6s) - 150, 300
- DTFC (DOGE) (build 682) & Cicada v2 20A (G_H_30) - 300
- DTFC (DOGE) & Littlebee 20A-S with cap removed (A_H_15) - 300, 600
- IMPULSERCF3 & ZTW polaris 30A

####Components tried and are not currently working:

- DYS XS20
- DYS XS30 with signal cap left on
- AIKON v2 20A
- DALRC 25A
- Littlebee_S 30A
- Spedix 25a blheli_s

####Components that will NOT likely ever work:
- Naze32 and clones
- All FCs with STM32F1