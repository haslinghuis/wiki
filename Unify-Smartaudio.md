## Setup

#### From teralift's post in Boris' thread

- Targets  
TBS SmartAudio is supported on all F3 and F4 targets (except for those with integrated VTX).

- Wiring  
Just wire the SmartAudio wire to a free hardware UART (TX) port.

- Configuration  
The up to date configurator supports easy configuration of the SmartAudio on the selected port.

1. Goto Ports tab
2. Select TBS SmartAudio from Peripherals drop down menu
3. Speed can be left at AUTO.

![Enabling SmartPort in Peripherals](https://cloud.githubusercontent.com/assets/14850998/22005655/804c7c26-dca8-11e6-80b4-3c67765dc0e3.png)

- Generic CMS  
3.1 will come with the generic CMS (Configuration Menu System) that runs on top of multiple display devices;  FC-integrated OSD, I2C OLED display and external OSD (MinimOSD variants) running latest version of MWOSD (Release 1.6.5 or later).  
(You even can switch between OSD and OLED while in CMS.)  
This means that users of external OSDs can control SmartAudio from the CMS.

####From AILERON8's post in Boris' thread:

There is a little info on smartaudio setup in here, but you may need to do a bit of troubleshooting to make it work for your particular setup. Good luck! I look forward to setting up SmartAudio on my next BFF3 quad myself.

https://github.com/betaflight/betaflight/issues/1029

http://www.team-blacksheep.com/tbs-u...5g8-manual.pdf   

####Here is a tutorial by Amano13:  
https://tmr.kiwi/betaflight-mwosd-smartaudio-cms/

####From Boris:  
The easiest is of course to get fc with OSD. That works absolutely flawless, but
there is a separate betaflight repository with LUA scripts
https://github.com/betaflight/betaflight-tx-lua-scripts

I am thinking to add more howto videos to github locations. So those who are willing to make nice howto videos please post it in here.
## User's Responsibility

The SmartAudio support unlocks certain capabilities of a SmartAudio device, to provide users with maximum flexibility. Therefore, it is user's responsibility to operate the device within the limits of respective local regulations.

## Compatibility

- The SmartAudio support was developed and tested with SmartAudio V2 devices (Unify 5G8 Pro and Unify 5G8 Pro HV). If you have a SmartAudio V1 device and have a problem, please report it using the issue facility in the main github repo page for Betaflight.
- Unify 5G8 Pro Race Edition:
Lower frequencies are not supported.
Power setting can be selected as 500 or 800, but will only go up to 200, as will be indicated on the status line.
- SPARKY2:  
Due to the pull-up resistors, Flexi-port is not suitable for Unify 5G8 Pro, Pro HV and Pro HV Race edition.
Main-port may be compatible (need testing).

## SmartAudio CMS guide

### The top menu
The top menu for SmartAudio VTX looks like this.
![SmartAudio CMS Top menu (Band/Chan mode)](https://cloud.githubusercontent.com/assets/14850998/21961195/c2639562-db46-11e6-9f98-71d54f6a879b.jpg)
While most of the entries are intuitive, there are several things that need additional explanation.

### Status Line

The status line on the top menu page of SmartAudio VTX menu indicates current status of the vtx in the following format:

```
m bc ffff ppp
```

where

`m` : Operational model, `F` (Freestyle) or `R` (Race).

`b` : Current transmitting band, `A` (BOSCAM A), `B` (BOSCAM B), `E` (BOSCAM E), `F` (FatShark/NexWave) or 'R' (Raceband).

`c` : Current transmitting channel, `1` through `8`.

`ffff`: Current transmitting frequency.

`ppp`: Current transmitting RF power, numeric value for mW (`25`, `200`, `500`, `800`), or `PIR` (In-Range Pit mode) or `POR` (Out-Range Pit mode).

Note that the status line indicates "running" status of the VTX device, and values may be different from band, channel and power setting entries below the status line.

### Operational Models
In Betaflight, a SmartAudio device operates in one of two operational models:

#### Race 
This is a model that gives minimum interferance to other pilots.
A SmartAudio device powers up in pit mode, and remain in pit mode until transmission is commenced.

When operating in this model, left most character of the status line is `R`.
If the device is in pit mode, current power field of the status line is either `PIR` or `POR`, until transmission is commenced.

While in the pit mode, changes to `BAND`, `CHAN` and `POWER` will not take effect until `SET` menu entry and associated confirmation is done (transmission commencing).
`BAND`, `CHAN` and `POWER` can be modified after commencing, but they all still require `SET` to take effect.

Refer to TBS Unify 5G8 Pro Manual for explanation of "In-Range" and "Out-Range".

#### Freestyle 
This is a model used when flying alone. A SmartAudio device will power up actively transmitting at band and channel with power as they were set before the power cycle.
When operating in this model, left most character of the status line is `F`,
and current power field matches that of power level selection menu entry.
Changes to `POWER` takes effect immediately, but changes to `BAND` and `CHAN` must be commenced by `SET`.

#### Switching between Freestyle and Race
There is an `OPMODEL` entry in the `CONFIG` sub-menu. Select either `FREE` or `RACE`. A device must be power cycled for the change to take effect.

### CONFIG sub-menu

![SmartAudio CMS CONFIG submenu](https://cloud.githubusercontent.com/assets/14850998/21961345/de0b760a-db4a-11e6-8309-abc6227ddc7c.jpg)

#### OP MODEL
Selection between race operational model (`RACE`) and freestyle operational model (`FREE`).  
Requires power cycle to take effect.

#### FSEL MODE
Frequency selection method. Requires power cycle to take effect.
- Channel mode ('CHAN'): Frequency is selected by specifying band and channel.
- Frequency mode ('FREQ': Frequency is specified by numerical value in MHz.  
When set to frequency mode, operational model is automatically set to freestyle, and top menu will be altered to enable direct frequency adjustment.

#### PIT FMODE
Specifies frequency to use while in pit mode. Requires power cycle to take effect.
- In-Range (`PIR`): Pit mode frequency is specified by band and channel set before the power cycle.
- Out-Range (`POR`): Pit mode frequency is specified by the value of `POR FREQ`.

_*WARNING*_  
Do not change this entry to POR without VRX capable of receiving at frequency specified by the `POR FREQ` entry.
If you do without such VRX, you will be blinded until Out-Range pit mode is cleared.

#### POR FREQ
Specifies frequency to use while in _Out-Range_ pit mode.  

#### STATX
Protocol statistics between a flight controller and a SmartAudio device. May help you to trouble shoot connection problems.

### Trouble shooting
#### Recovery from accidental Out-Range pit mode
- You can cancel pit mode by button operation. Refer to the Unify manual.
- You can use alternative CMS device such as I2C OLED to cancel the Out-Range mode.
- You can tap or connect VIDEO OUTPUT from OSD and connect it to external display or goggle's VIDEO INPUT.