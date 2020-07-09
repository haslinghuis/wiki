## Overview

- _**Pinio**_ is an abstraction of simple GPIO (General Purpose I/O) pin facility, and _**piniobox**_ is a facility to associate boxes (modes) to `pinio`.
- Most targets are build to be capable of configuring up to four pins for the pinio facility, which can then be controlled by the piniobox facility.

## Pinio

An MCU pin can be assigned to `pinio` by `resource` CLI command.
```
resource pinio <index> <pinid>
```
Example of `resource pinio`
```
resource pinio 1 A1
resource pinio 2 A8
resource pinio 3 C9
resource pinio 4 D2
```

I/O configuration of each pin is specified by `pinio_config` CLI variable, which is a comma separated list of 8-bit values. MSB represents inversion, and remaining 7-bits specify I/O mode as defined in drivers/pinio.h (Only push-pull output is defined for this PR).
```
#define PINIO_CONFIG_OUT_INVERTED 0x80
#define PINIO_CONFIG_MODE_MASK    0x7F
#define PINIO_CONFIG_MODE_OUT_PP  0x01
```

Example of `pinio_config`
```
set pinio_config = 1,129,1,1
```
sets Output-Push-Pull for `pinio` 1, 3 and 4, and Inverted Output-Push-Pull for `pinio` 2. Default value is 1 (Output-Push-Pull).

## PinioBox

CLI variable `pinio_box`, comma separated list of permanent ID of boxes, associate the boxes to corresponding `pinio`s. Once associated, the boxes's activation status are reflected to the associated `pinio`s (and then to pins). Since the piniobox facility has it's own capability of monitoring activation state of boxes, it operates independently from what boxes are meant for. (In other words, the piniobox _**adds**_ pinio capability to boxes.)

Permanent IDs 40 through 43 are user defined boxes, which are activated and appear as `USER1` through `USER4` in the list of boxes at places such as Modes tab in configurator.

Example of `pinio_box`
```
set pinio_box = 0, 39, 43, -1
```
With this assignment, `pinio` 1 through 4 are associated with boxes as follow.

| `pinio` | Box | Permanent ID |
|---|---|---:|
|`1` | `ARM` | `0` |
|`2` | `VTX PIT MODE` | `39` |
|`3` | `USER4` | `43` |
|`4` | Unused | `-1` |

For permanent ID of boxes, the table below is based on `msp/msp_box.c`.

 | Box | Description | ID | Notes |
 | --- | --- | ---: | --- |
 | BOXARM | ARM | 0 | |
 | BOXANGLE | ANGLE | 1 | |
 | BOXHORIZON | HORIZON | 2 | |
 | BOXBARO | BARO | 3 | |
 | BOXANTIGRAVITY | ANTI GRAVITY | 4 | |
 | BOXMAG | MAG | 5 | |
 | BOXHEADFREE | HEADFREE | 6 | |
 | BOXHEADADJ | HEADADJ | 7 | |
 | BOXCAMSTAB | CAMSTAB | 8 | |
 | BOXCAMTRIG | CAMTRIG | 9 | (removed) |
 | BOXGPSHOME | GPS HOME | 10 | (removed) |
 | BOXGPSHOLD | GPS HOLD | 11 | (removed) |
 | BOXPASSTHRU | PASSTHRU | 12 | |
 | BOXBEEPERON | BEEPER | 13 | |
 | BOXLEDMAX | LEDMAX | 14 | (removed) |
 | BOXLEDLOW | LEDLOW | 15 | |
 | BOXLLIGHTS | LLIGHTS | 16 | (removed) |
 | BOXCALIB | CALIB | 17 | |
 | BOXGOV | GOVERNOR | 18 | (removed) |
 | BOXOSD | OSD DISABLE SW | 19 | |
 | BOXTELEMETRY | TELEMETRY | 20 | |
 | BOXGTUNE, | GTUNE | 21 | (removed) |
 | BOXRANGEFINDER | RANGEFINDER | 22 | (removed) |
 | BOXSERVO1 | SERVO1 | 23 | |
 | BOXSERVO2 | SERVO2 | 24 | |
 | BOXSERVO3 | SERVO3 | 25 | |
 | BOXBLACKBOX | BLACKBOX | 26 | |
 | BOXFAILSAFE | FAILSAFE | 27 | |
 | BOXAIRMODE | AIR MODE | 28 | |
 | BOX3D | DISABLE / SWITCH 3D | 29 | |
 | BOXFPVANGLEMIX | FPV ANGLE MIX | 30 | |
 | BOXBLACKBOXERASE | BLACKBOX ERASE (>30s) | 31 | |
 | BOXCAMERA1 | CAMERA CONTROL 1 | 32 | |
 | BOXCAMERA2 | CAMERA CONTROL 2 | 33 | |
 | BOXCAMERA3 | CAMERA CONTROL 3 | 34 | |
 | BOXFLIPOVERAFTERCRASH | FLIP OVER AFTER CRASH | 35 | |
 | BOXPREARM | PREARM | 36 | |
 | BOXBEEPGPSCOUNT | BEEP GPS SATELLITE COUNT | 37 | |
 | BOX3DONASWITCH | 3D ON A SWITCH | 38 | (removed) |
 | BOXVTXPITMODE | VTX PIT MODE | 39 | |
 | BOXUSER1 | USER1 | 40 | |
 | BOXUSER2 | USER2 | 41 | |
 | BOXUSER3 | USER3 | 42 | |
 | BOXUSER4 | USER4 | 43 | |
 | BOXPIDAUDIO | PID AUDIO | 44 | |
 | BOXPARALYZE | PARALYZE | 45 | |
 | BOXGPSRESCUE  GPS RESCUE | 46 | |
 | BOXACROTRAINER | ACRO TRAINER | 47 | |
 | BOXVTXCONTROLDISABLE | DISABLE VTX CONTROL | 48 | |
 | BOXLAUNCHCONTROL | LAUNCH CONTROL | 49 | |

## FAQ
- None yet.
