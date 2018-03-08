# RunCam Device Protocol
## Description
RunCam Device Protocol is the serial communication protocol for the RunCam HD Camera and RunCam analog camera.Through this protocol, users can use these devices more easily, for exemple,for the RunCam Split, if the current is cut off, and then we power the camera for recording again, inaccurate time of the camera will cause the creation time of media file to be inaccurate, we can solve this proble by this protocol;We can also display the available SD-Card space of the camera and other functions already available for the camera through Betaflight. This protocol will continue to be updated in the future, hoping you can control the RunCam camera more convenient.

## Protocol specification
@todo

***

## For FPV Camera: Swift 2, Micro Swift 2, Micro Eagle, ...
### OSD Cable Simulation on Transmitter
Through this function, with the RunCam Control Adapter(Comming Soon), for the most cameras with OSD Input Pin(yes, included the FPV camera not RunCam), we can operate the OSD menu of camera via the two joysticks of the transmitter, including some shortcut key settings. For exemple, for Micro Swift2, we can  push the Roll of the transmitter to the maximum value continuously(simulate the right button of the OSD) to switch the scene, so you can easily adjust the camera to adapt to the different light environment.

#### Setup Guide
![RC-CA_viki-manua](https://s3-us-west-2.amazonaws.com/runcamfcfiles/RC-CA_viki-manual_v3.jpg)

#### Part of camera list confirmed compatible
| Swift series | Compatible | | Eagle series | Compatible | | Sparrow series | Compatible | | Owl series | Compatible |
| --- | --- | --- | --- | --- | --- | --- |  --- | --- | --- | --- |
| Swift 2 | Yes | | Micro Eagle | Yes |  | Sparrow | Yes |  | Owl 2 | Yes | 
| Micro Swift 2 | Yes | |Eagle 2 Pro | Yes | | Micro Sparrow | Yes | | Owl | No |
| Swift 2 Rotor Riot | Yes |  |Night Eagle 2 | Yes | | | | | | |
| Micro Swift | Yes | | Eagle 2 | Yes | | | | | | | | 
| Swift Mini 2 | Yes | | Eagle | No | | | | | | |  
| Swift Mini | Yes | | | | | | | |

 
| Sky series | Compatible | | Nano | Compatible
| --- | --- | --- | --- | --- |
| SKYPLUS | Yes | | Nano | No |
| SKY | Yes | | | | 



***

## For HD Camera: RunCam Split 1, RunCam Split 2, RunCam Split mini
### Camera Button Simulation  
#### Preparation

Firmware: BetaFlight Firmware (≥3.2.0)
Configurator software: Betaflight Configurator  (≥3.2.0)
Any available UART interface on the BetaFlight
#### 1.Connect the RunCam Split with the UART interface of the Flight Controller

![split2_fc-01](https://s3-us-west-2.amazonaws.com/runcamfcfiles/split2_fc-01.jpg)


#### 2.Make the Flight Controller recognize the Split

For example, we connect the Split to the UART 3 interface on the BetaFlight: connect the flight controller to the computer, then open the Betaflight Configurator. 
In the Peripherals column of the line UART3 (on the Ports tab), select RunCam Device and click Save And Reboot.
![bf-ports-setup-for-rcsplit](https://s3-us-west-2.amazonaws.com/runcamfcfiles/bf-ports-setup-for-rcsplit.png)

#### 3.Instructions of the functions of the camera and assigning transmitter channels to them

In the Betaflight Configurator, navigate to the Modes tab. There are new CAMERA WI-FI, CAMERA POWER and CAMERA CHANGE modes.

CAMERA WI-FI: turn on/off the WIFI of the camera. When in the OSD of the camera, this is used to confirm your selection.
CAMERA POWER: start/stop the video. When in the OSD of the camera, this is used to move to the next menu item.
CAMERA CHANGE MODE: switch among the three modes, video, photo and OSD setting mode. When in the OSD of the camera, this will exit the menu.
Assign any available channel to the function you need, for example:

Assign the AUX1 to the CAMERA WI-FI, range 1900-2100
Assign the AUX2 to the CAMERA POWER, range 1900-2100
Assign the AUX3 to the CAMERA CHANGE MODE, range 1900-2100
![bf-modes-setup-for-rcsplit](https://s3-us-west-2.amazonaws.com/runcamfcfiles/bf-modes-setup-for-rcsplit.png)


#### 4.Assign the channel to the switch of the controller

Please choose your Model on the controller, then access to the Mixer interface and assign the channel to the switch of the controller. Take opentx 2.2.0 for example, assign the channels CH5, CH6, and CH7 to SA, SB, SD respectively.
![IMG_0371-1](https://s3-us-west-2.amazonaws.com/runcamfcfiles/IMG_0371-1.jpg)



#### 5.Test

 Power the Flight Controller and the RunCam Split

Set the SA to the bottom, the camera turns on/off the WIFI 
Set the SB to the bottom, the camera starts/stops the video
Set the SD to the bottom, the camera switches among the three modes: video, photo and OSD setting mode
 

#### View demonstration video:

https://goo.gl/tm8CPS