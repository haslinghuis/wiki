## What's new

- 2017-02-12 Note on compatibility of SmartAudio V1 devices (Compatibility section)

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

http://team-blacksheep.com/tbs-unify-pro-5g8-manual.pdf   

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

- The SmartAudio support was developed and tested with SmartAudio V2 devices (Unify 5G8 Pro and Unify 5G8 Pro HV).

- The SmartAudio V1 may not be compatible with hardware UARTs (as of 2017-02-12). If you have trouble with V1 devices with hardware UARTs, please try the software serial (will be available in v3.1.6 patch release, schedule to be released on 2017-03-01).

- Unify 5G8 Pro Race Edition:
Lower frequencies are not supported.
Power setting can be selected as 500 or 800, but will only go up to 200, as will be indicated on the status line.

- SPARKY2:  
Due to the pull-up resistors, Flexi-port is not suitable for Unify 5G8 Pro, Pro HV and Pro HV Race edition.
Main-port may be compatible (need testing).

## SmartAudio CMS guide

### The top menu (Band/Channel mode)
The top menu for SmartAudio VTX in band/channel mode looks like this.
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

### The top menu (Band/Channel mode)

When a SmartAudio device is in user frequency mode, the SmartAudio CMS Top Menu looks like this.
![SmartAudio CMS Top menu (Frequency mode)](https://cloud.githubusercontent.com/assets/14850998/22690953/7ac836ee-ed7b-11e6-8c71-139f1eb919aa.png)
It allows direct entry of arbitral frequency between 5600 and 5900MHz, by selecting the `FREQ` entry, which will take to a submenu like this.

![SmartAudio Frequency selection menu](https://cloud.githubusercontent.com/assets/14850998/22690983/a8db502a-ed7b-11e6-9570-e2f406f5d29b.png)

The `NEW FREQ` allows selection of a new frequency, and `SET` will commence the transmission at the frequency. Accelerating auto repeat can be used here to prevent you from grounded when making a large change.

#### Switching between band/channel mode and user frequency mode

To switch from band/channel to user frequency mode:<br>
(1) Navigate to `SA CONFIG` menu.<br>
(2) Change `OP MODEL` to `FREE` if not already `FREE`.<br>
(3) Change `FSEL MODE` to `USER`.<br>
(4) Power cycle the SmartAudio device (You don't have to power cycle the FC).<br>
(5) Navigate back to SmartAudio VTX top menu.

To switch from user frequency mode to band/channel mode:<br>
(1) Navigate to `SA CONFIG` menu.<br>
(2) Change `FSEL MODE` to `CHAN`.<br>
(3) Power cycle the SmartAudio device (You don't have to power cycle the FC).<br>
(4) Navigate back to SmartAudio VTX top menu.<br>

### CONFIG sub-menu

![SmartAudio CMS CONFIG submenu](https://cloud.githubusercontent.com/assets/14850998/21961345/de0b760a-db4a-11e6-8309-abc6227ddc7c.jpg)

#### OP MODEL
Selection between race operational model (`RACE`) and freestyle operational model (`FREE`).  
Requires power cycle to take effect.

When race operational model is selected, the frequency selection mode (`FSEL MODE`) will automatically set to `CHAN`. This is by the specification of the current hardware (Unify 5G8 Pro/Pro HV/Race).

#### FSEL MODE
Frequency selection method. Requires power cycle to take effect.
- Channel mode ('CHAN'): Frequency is selected by specifying band and channel.
- Frequency mode ('FREQ'): Frequency is specified by numerical value in MHz.  
When set to frequency mode, operational model is automatically set to freestyle, and top menu will be altered to enable direct frequency adjustment.

The Frequency mode (`FREQ`) is only available when the operational model is freestyle (`FREE`). To choose the frequency mode, first switch the operational model to free style.

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

## Modify VTX Settings (TBS Unify / TrampHV) using FrSky TARANIS Menu   
http://www.nitbeatfpv.com/tramphv-unify-vtx-settings-taranis

Note: The bf script linked in this 'how to' link isn't the latest one. You need the one linked here.   
https://github.com/betaflight/betaflight-tx-lua-scripts  

### Tarnis upgrading and setup
Originally Posted by elmattbo:  
Ensure opentx 2.2 on the radio and avoid rc11. If upgrading from 2.1 you will have to copy in the new file structure for 2.2 to the sd card - back your sd card up first.
The early bf 3.1 releases do have SmartAudio selectable in the ports tab but don't necessarily work, so update to the latest bf.
Install the lua scripts linked from the bf wiki (x7, x9 as appropriate) into the scripts folder on the Taranis sd card.
Attach the vtx audio lead to either uart 1 or 3 tx pin (RG SSD FC - Connect to the UART available on the FC you use).
Select tbs SmartAudio for the appropriate UART in the ports tab.
Go to display in the Taranis and set a screen to 'scripts' it should then offer you the lua script you installed earlier.
From the main model screen a long press of page will bring up the bf scripts and you press menu to cycle to the vtx screen. 
Update from elmattbo:  
So after trying it today I found that the vtx didn't respond to changes in the lua script. Tried a power reboot after changing settings but it made no difference. I suspect I'm missing something in the set up, but it could be that the taranis reads the vtx, but doesn't write any changes. I have read that the half wave duplex (or whatever it is) communications protocol took a little figuring out for the devs so the issue may be in betaflight too.  

Fixed! I hadn't saved the settings with a long press of the Taranis menu.  
Change VTX Settings
Store or Reload values - Long Press MENU button
Switch screens - Short Press MENU button
Navigate between values - "+" & "-" buttons
EDIT value - Press ENTER
