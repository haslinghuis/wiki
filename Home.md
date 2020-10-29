![BetaFlight](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/betaflight/bf_logo.png)
# Welcome to the BetaFlight Wiki!

**New to Betaflight and just want to get started? See the [installation](Installing-Betaflight) section.**

This Wiki is available in other languages: [简体中文](https://airfleetteam.gitbook.io/bf-wiki-chinese/)


## Events

| Date  | Event |
| - | - |
| 20 Oct 2020 | Latest [release](https://github.com/betaflight/betaflight/releases/tag/4.2.4) of Betaflight 4.2 |

## News

To get the latest update from us, you can now also visit our webpage at https://betaflight.com/.


## Merchandise:
**For a limited time only:** Buy a [Betaflight Shirt or Hoodie](Betaflight-merchandise)!


## Wiki Tips:   
- Searching the Wiki-  
Type in URL bar of the Web Browser:   
"site:github.com/betaflight/betaflight/wiki PT1" (wiki search for term 'PT1')  
or   
"site:github.com/betaflight/betaflight PT1" (broader search for term 'PT1')   

-  Note that Default settings as well as CLI commands may change from one Version to the next. We try to list these under the Version Release Notes but not always get them all. a method to use to keep track of changes and to check these yourself is:    
After Flashing but before configuring do the CLI Dump then Copy/paste this into a Text file named with V#. Then on next update do this again to a new text file.  
This allows you to check for differences by either manually compared the two text files or by using an Difference option in a text editor.  NotePad++ is one such editor with a good difference feature.

- If not in this Wiki please try the [docs](https://github.com/betaflight/betaflight/tree/master/docs) directory as another source for information. Note: includes some documentation from Cleanflight

- All BASICs and alternate methods to config and flash are in the NAZE32 Manual here: http://www.abusemark.com/downloads/naze32_rev2.pdf  

## Introduction
Betaflight is software that is open source and is available free of charge without warranty to all users.

Betaflight is a Cleanflight fork started by BorisB. It used to work as a beta test platform for Cleanflight and kept pushing the envelope in terms of performance, but it eventually evolved into a fully grown up, stable and well maintained firmware by various developers. In fact it looks like it is the number 1 open source multi-rotor firmware according to Google Trends analysis around January 2017.

The name "Beta" comes from the fact it started with a goal to keep trying new things out and have quick and easy test distributions available. The name "Beta" has stuck as the brand name was strong at the moment where it transitioned from beta to stable and there was already a lot of documentation around.

"This project also helps by contributing to other open source projects like iNav." - says Boris B ([lead developer](http://www.youtube.com/user/bozic1982/featured))

Some interactive statistics on how Betaflight is used:
[![Betaflight Statistics](images/betaflight_statistics.jpg)](https://datastudio.google.com/s/kfHdPaVFYUU)


## Tools
Betaflight is also always being adjusted to support most current Cleanflight tools like Configurator and EzGui devices and many other MSP tools. There is no special tool needed just for Betaflight.

## Firmware Releases
Releases can be found here: https://github.com/betaflight/betaflight/releases or downloaded from configurator.
Also check the Upgrading List to the Right for Release Notes and other Details on the various Versions.

*BETA TESTING (WARNING)*

*If you want to contribute to better development you can download the latest beta build directly from:* *https://ci.betaflight.tech/job/Betaflight/lastBuild/artifact/obj/*

Contributors create [pull requests](https://github.com/betaflight/betaflight/pulls), or PR's.  Pre-built hex files for the four main unified targets can be downloaded by:
- scrolling to the bottom of the PR, 
- at `All checks have passed`, click `Show all checks` at right,
- at `Azure Pipelines` on left, click betaflight.betaflight, and then `View more details on Azure Pipelines` in small grey print,
- at the bottom, under `Build`, click `1 Artefact`,
- hover over the betaflight line, a three vertical dot dropdown appears at right, click on that and choose `Download artefacts`
You should get a zip file containing hex files of the PR.

You can find release planning here:   
https://github.com/betaflight/betaflight/milestones

## Configuration Tool
To configure Betaflight you should use the latest stable Betaflight-configurator GUI tool (Windows/OSX/Linux) that can be found in the Betaflight Configurator repository:  
https://github.com/betaflight/betaflight-configurator/releases

*BETA TESTING*

*If you want to contribute to better development you can download the latest beta build from:*
*https://github.com/betaflight/betaflight-configurator-nightlies/releases*

## BlackBox Viewer
Viewer Releases are here:
https://github.com/betaflight/blackbox-log-viewer/releases

The Latest Viewer source is here:   
https://github.com/betaflight/blackbox-log-viewer  
See [BB Logging and Usage](Black-Box-logging-and-usage) Wiki page on using the BB logger. 

## BetaFlight Logos
Links to logos (Note: The bee/wasp in the logo is different from the one that was finally adopted):  
https://www.rcgroups.com/forums/showpost.php?p=34909081&postcount=29679

Here the original ones designed by skaman82:  
https://www.dropbox.com/s/viczizbjz0fwod4/Betaflight.Logos.zip?dl=0

Or The logo bf uses is at the top of this wiki page, just mouse right and save image as, cut the writing if you don't want it 

## Other Firmware that runs on same hardware as BF and has related source code:   
Official CleanFlight documentation: http://github.com/cleanflight/cleanflight/wiki

Check out the INAV project. It's focused on GPS/Alt Hold/Autonomous flight.  
http://inavflight.com  

## Providing feedback and contributing to this project
Visit this RC Groups Forum to join the discussion: http://www.rcgroups.com/forums/showthread.php?t=2464844

Financial support to the Betaflight Team by PayPal donation:

[![Donate](https://www.paypalobjects.com/en_US/NL/i/btn/btn_donateCC_LG.gif)](https://paypal.me/betaflight)
