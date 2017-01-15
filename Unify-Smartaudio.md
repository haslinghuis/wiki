## Setup

#### From teralift's post in Boris' thread

- Targets  
TBS SmartAudio is supported on all F3 and F4 targets (except for those with integrated VTX).

- Wiring  
Just wire the SmartAudio wire to a free hardware UART (TX) port.

- Configuration  
As couples of previous post noted, the up to date configurator also supports easy configuration of the SmartAudio on the selected port; Ports tab, Peripherals column, SmartAudio, speed is ignored.

- Generic CMS  
3.1 will come with the generic CMS (Configuration Menu System) that runs on top of multiple display devices;  FC-integrated OSD, I2C OLED display and external OSD (MinimOSD variants) running latest version of MWOSD. (You even can switch between OSD and OLED while in CMS.)  

This means that users of external OSDs can control SmartAudio from the CMS.

####From AILERON8's post in Boris' thread:

There is a little info on smartaudio setup in here, but you may need to do a bit of troubleshooting to make it work for your particular setup. Good luck! I look forward to setting up SmartAudio on my next BFF3 quad myself.

https://github.com/betaflight/betaflight/issues/1029

http://www.team-blacksheep.com/tbs-u...5g8-manual.pdf   

####Here is a tutorial by Amano13:  
https://tmr.kiwi/betaflight-mwosd-smartaudio-cms/

## User's Responsibility

The SmartAudio support unlocks certain capabilities of a SmartAudio device, to provide users with maximum flexibility. Therefore, it is user's responsibility to operate the device within the limits of respective local regulations.

## Compatibility

- The SmartAudio support was developed and tested with SmartAudio V2 devices (Unify 5G8 Pro and Unify 5G8 Pro HV). If you have a SmartAudio V1 device and have a problem, please report it using the issue facility in the main github repo page for Betaflight.
- Unify 5G8 Pro Race Edition:
Lower frequencies are not supported.
Power setting can be selected as 500 or 800, but will only go up to 200, as will be indicated on the status line.

## SmartAudio CMS guide
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
While in the pit mode, changes to band, channel and power will not take effect until SET menu entry and associated confirmation is done (transmission commencing).
Band, channel and powers can be modified after commencing, but they all still require SET to take effect.

When operating in this model, left most character of the status line is `R`.
If the device is in pit mode, current power indicator of the status line is either `PIR` or `POR`, until transmission is commenced.
Refer to TBS Unify 5G8 Pro Manual for explanation of "In-Range" and "Out-Range".

#### Freestyle 
This is a model used when flying alone. A SmartAudio device will power up actively transmitting at band and channel with power as they were set before the power cycle.
When operating in this model, left most character of the status line is `F`,
and current power indicator matches that of power level selection menu entry.
Changes to power takes effect immediately, but changes to band and channel must be commenced by SET.

#### Switching between Freestyle and Race
There is a OPMODEL entry in the CONFIG sub-menu. Select either `FREE` or `RACE`. A device must be power cycled for the change to take effect.