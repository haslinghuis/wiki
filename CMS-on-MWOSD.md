## BetaFlight CMS and MWOSD Canvas Mode Guide

--

> scarab-osd (MWOSD) 1.6.7 has a bug that causes single asterisk (*) to show up at the upper left corner of the screen and goes on to silence when CMS is invoked.  

> It has been fixed with 1.6.7.1.

> The 1.6.7 was available approximately between 2017-01-17 to 2017-01-29. Anyone who downloaded and flashed this version should update it to 1.6.7.1. Apologies for your inconvenience.

--


Starting with Betaflight v3.1.0, flight controller (FC) will have a Configuration Menu System that operates across number of different display devices, such as FC-integrated OSD, external OSD that understands new MSP_DISPLAYPORT message, OLED display and radio telemetry screen.

MWOSD Release 1.6.5.0 and later supports the CMS with CANVAS extension.

[A tutorial](https://tmr.kiwi/betaflight-mwosd-smartaudio-cms/)

### FC configuration

FC firmware must be built with `CMS` and `USE_MSP_DISPLAYPORT` or equivalent options, and corresponding features should be turned on in configuration if they are controlled via features.

BetaFlight v3.1.0 for targets F3, F4 and F7 (except for those with FC-integrated-SPI-connected OSD) all have these turned on, and ready for CMS running on top of MWOSD.

### MWOSD configuration

MWOSD release is available at  
[https://github.com/ShikOfTheRa/scarab-osd/releases](https://github.com/ShikOfTheRa/scarab-osd/releases)

Please refer to ubiquitously available "How to download, configure and flash MWOSD" write-ups and video clips for basic how-to.

To configure the MWOSD for CMS, `CANVAS_SUPPORT` option in `Config.h` must be enabled.  
`BETAFLIGHT` option, which always means "most recent release" will do this automatically.

### CMS activation

Stick command to activate the BetaFlight CMS is `Thr MID + Yaw LEFT + Pitch UP`.

(Notice that MWOSD menu activation is `Thr MID + Yaw RIGHT + Pitch UP`.

### Caveats
          
#### Canvas Mode with full DISPLAYPORT
MWOSD can work with full DISPLAYPORT support (configure OSD through Betaflight Configurator OSD tab), however there are limitations.  At 60Hz update rate, 115.2Kbps (roughly 11KB/s) is only capable of supporting 190 characters per cycle. Any display item beyond this bandwidth limit will build up in the transmit queue, and as the queue builds up, newest characters will be delayed, and will eventually start to drop, making overlay look slow and jittery.  This may be acceptable if only using limited or static OSD elements like Voltage/Current monitoring and Timer, however things like artificial horizon indicator may lag and jitter and become unusable.

To use with DISPLAYPORT, simply update to the latest firmware on your MWOSD, and build your firmware target with added options (example for REVOLT target):
```bash
make OPTIONS="USE_OSD USE_OSD_OVER_MSP_DISPLAYPORT" REVOLT
```

This will enable the OSD tab and full DISPLAYPORT support from the FC.

#### Dual Menu Chaos 
        
When the MWOSD is used together with FC-side CMS, you are going to have two menu systems; one in the FC and one in MWOSD. It would certainly be an awful experience if you activate both menus simultaneously.

MWOSD has two measures built-in to avoid the dual menu chaos:
    
1. MWOSD's menu will not be invoked if FC-side CMS is active.

2. MWOSD's menu will QUIT immediately.

In addition, CMS invocation stick commands are configured to be different between BetaFlight and MWOSD, but you must still be careful not to activate one while another is active.

#### OOS (Out-Of-Sync)

MWOSD is very stable, and so is the canvas mode support.

However, since the canvas mode protocol is simplex from FC to MWOSD, CMS on FC and MWOSD may get out-of-sync in a rare case, such as resetting or power cycling the MWOSD while the CMS is active.

You can tell the out-of-sync state by:

1. If you power cycle or reset MWOSD while in CMS, then MWOSD may not get out of opening screen.
2. You may see an asterisk character ('*') at upper left corner of your screen when this happens.
3. You may also see cursor character move as you input navigational stick commands.
4. Other erratic text displayed (not a screen full of random characters).

There are numbers of ways to get out of this state.

1. Enter a stick command that causes page redraw, such as menu back. (It is not a wise move to enter a stick command that causes item selection.)
2. Blindly navigate to BACK or EXIT menu item and select it.
3. Reset or power cycle your flight controller.

Happy Flying!