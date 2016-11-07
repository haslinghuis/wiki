# Dshot & BetaFlight   
Dshot is new and currently code is being developed and tested. Since there are many dependencies and not all current hardware works this is to help keep track.

The Official Thread for Dshot is: https://www.rcgroups.com/forums/showthread.php?t=2756129
Please Read this thread for details of how it works and up to date info.

BetaFlight development code for Dshot can be found here:
http://andwho.sytes.net:8080/job/BorisB_BetaFlight/
Just remember that this is very Experimental code and may have serious limitations.

Some known limitations are:
- BLHeli pass-through does not work if FC is set to DSHOT. Must change output to OneShot to use the pass0through.
- on some Targets BB logging, and a few other features do NOT work yet.

###Starting with a post by mxracer33x in the Dshot thread:

Scouring the BF and Dshot threads has resulted in the following lists. The information posted herein is all provided on an AS-IS basis.
It has been derived from posts on this forum, and direct correspondences with users.

If you have more info, please PM me with all the details you can, and Ill edit list accordingly.

USE THE FOLLOWING INFORMATION AT YOUR OWN RISK

BETAFLIGHT AND Dshot -

BEtaflight may not be able to support all Targets ( or specific boards) due to the design of each one differing on the DMA pinout. Look below to find what may or may not be working.

DSHOT on BLHELI-S ESCs will oinly support Dshot150 & 300 Heres why
https://www.rcgroups.com/forums/show...&postcount=376

UPDATED 11-7-2016 @ 9:30am PST

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

####Intermittent FC Reports:
- KOMBINI
- SPracingF3

####ESCs supporting Dshot:
- KISS 24 - 150, 300, 600 See the Dshot thread (link above) for ESC code.

####BLheli-S:

- Racestar v1 30A
- Racestar v2 20A & Cicada 20A - 300
- Racestar v2 30A & Cicada 30A - 150
- Racerstar 25a - 300
- Aikon SEMF 30A v2 - 150
- Lumenier 30A
- TBS 25 - 300
- DYS XS30 (with Signal cap removed) https://www.rcgroups.com/forums/show...&postcount=836


####Combos (FC & ESC) working:

- KISSFC & KISS 24 - 150, 300, 600
- KISSFC & Racesta/CIcada 20A - 150
- OMNIBUS (with blackbox SD feature off) & KISS24
- OMNIBUSF4 & KISS24
- REVO & KISS24
- REvo & AIKON 30A
- BLUEJAYF4 & KISS24
- BlueJayF4 & Aikon Semf 30
- BlueJayF4 & v2 Racestar/Cicada 20a
- BlueJayF4 & v2 Racestar/Cicada 30a (1 report of BJF4 and Racestar v2 30a not working)
- Kombini & TBS 25 - 300
- Xracer 3.1 SPI & v2 Racestar/Cicada 20a - 150, 300
- FLIP32F4 & Racerstar25A - 150, 300
- LUXFC & Luminier 30A - 300
- LUXFC & v2 Racestar/Cicada 30A - 150
- FURY F4 & KISS24 - 600
- FURY F4 & v2 Racestar/Cicada 20a - 150, 300

Components tried and are not currently working:

DYS XS20
DYS XS30 with signal cap left on

Components that will NOT likely ever work: