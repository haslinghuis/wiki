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

## Compatibility

- The SmartAudio support was developed and tested with SmartAudio V2 devices. If you have a SmartAudio V1 device and have a problem, please report it using the issue facility in the main github repo page for Betaflight.
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

`m` : Operational mode, `F` (Freestyle) or `R` (Race).

`b` : Current active band, `A` (BOSCAM A), `B` (BOSCAM B), `E` (BOSCAM E), `F` (FatShark/NewWave) or 'R' (Raceband).

`ffff`: Current transmitting frequency.

`ppp`: Current RF power mode, numeric output power (`25`, `200`, `500`, `800`), or `PIR` (In-Range Pit mode) or `POR` (Out-Range Pit mode).
