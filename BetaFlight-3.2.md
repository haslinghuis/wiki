IMPORTANT ANNOUNCEMENT

## Betaflight v3.2 will be the last version to include STM32F1 based flight controllers. From v3.3 onwards support for those flight controllers will no longer be provided, this includes the NAZE, CC3D (original), ALIENFLIGHTF1 and MICROSCISKY and their clones.

If you have any questions or concerns please advise in Boris' BetaFlight thread. Providing notice here so that there is plenty of time to prepare. v3.2 will be released in October, whilst v3.3 won't be until next year.

You can see some information here : https://github.com/betaflight/betaflight/projects/2 however it doesn't look complete.

From Boris (30 June 2017):   
Well release candidate is around the corner.
Basically a lot of fundamental changes. Not much of those will affect the user, but there are also quite some new features as well.   
The major complain about Betaflight was too frequent updates in the past. So we slowed that down. Not having the pressure for quick releases gives some time for thinking and researching new things.  

Current V3.2 files are here: https://ci.betaflight.tech/job/Betaflight/lastSuccessfulBuild/artifact/obj/ 
or https://betaflight.qmd.cl/

### Note: When discussing this Version in the Forums (Boris' BetaFlight thread)   
## Please State: Version and Build Number.


### From Boris:  
Hi Guys. I hope everyone is fine.

We finally have a release candidate on schedule like promised!   
https://github.com/betaflight/betaflight/releases


## Betaflight 3.2.0 RC2 Pre-release

### New:

- Full F7 support (@sambas @blckmn )
- SITL simulator support (@cs8425)
- Added crosshairs to CMS (@lostcontrol)
- Improved ITerm windup handling for tricopter (@martinbudden)
- Added SP RACING F3 OSD/PDB suport (@hydra)
- Added horizon_tilt_effect command (@ethomas997)
- Improved scheduler efficiency (@lilcw)
- Increased motor output resolution (@borisbstyle)
- Improved pwm timer precision (@blckmn)
- Added new motor protocol Proshot1000 (@TonyBazz)
- Improved configuration architecture (PG implementation @martinbudden , @ledvinap)
- OSD improvements
- New target support
- Added Automatic Notch filter based on noise frequency (@rav-rav , @martinbudden)
- Fix/enable disabling of rc smoothing in level modes
- Flip inverted quad on ground (anti-turtle mode) (@brycedjohnson)
- Improved blackbox storage to be more compact. Allows recording at higher rates and/or longer logs on flash storage (@martinbudden)
- Camera control
- TBS compatible LED frequency indicator
- beeper / OSD / CLI indication of reason for not arming;

### Experimental - use with caution

- Added experimental crash detection and recovery (@martinbudden)
- Fixed full throttle drift when airmode disabled
- Added experimental CPU overclock options for F4
- FrSky SPI RX support

### Fixes:

- Fix GPS serial overflow (@mikeller)
- Fixed destabilisation on full throttle when Airmode disabled

### RC2 Changes:  
- Fix for new blackbox denominators
- Improved ACC scaling in some cases  

### Other Features:   

- Spektrum Satellite Bind for 3.2  
https://github.com/betaflight/betaflight/wiki/Spektrum-Satellite-Bind-for-3.2

- Reconfigurable Barometer for 3.2  
https://github.com/betaflight/betaflight/wiki/Barometer-Configuration-(3.2)


## Black Box Viewer
https://github.com/betaflight/blackbox-log-viewer/commits/master
