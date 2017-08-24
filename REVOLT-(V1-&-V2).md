# REVOLT V2 OWNERS TAKE NOTE

All of the information referring to Revolt V2 applies to Skitzo FC as well, it's identical board (just different color).

Is not currently supported in current BetaFlight builds but can run BetaFlight now with the following instructions.

1) Install 3.1.7 or higher Betaflight version

2) Solder the pads on the bottom of the board to enable UART 1 for Inversion See this ![Photo.](https://static.rcgroups.net/forums/attachments/4/7/1/6/9/4/a9783283-253-Revolt_V2%20Bottom.jpg)

3) Solder up the cable for the receiver on UART 1 as shown in the revolt V2 diagram (make sure its the V2 diagram)
https://revoltfc.com/pinout-diagram.html

4) Do this command in CLI.  
`set serialrx_halfduplex=ON`

5) Hit Save.

Enjoy 

#### Telemetry to FrSky XSR
As far telemetry you have to do the "un-inverted XSR hack" which isn't a big deal at all. 
[Link to the X4r/XSR hack](https://blck.mn/2016/06/smartport-the-frsky-xsr-and-betaflight/)
 the telemetry works with this XSR hack, just remember to connect your S.Port wire to TX3 pin on the Revolt v2, don't get confused with the Revolt v2 wiring diagram which shows S.Port going to Rx3 pin.

#### Blackbox logs download corruption fix for MAC users
If you have issues downloading blackbox logs, this will fix the problem :
https://www.rcgroups.com/forums/showpost.php?p=36811734&postcount=44503
more details here :
https://github.com/betaflight/betaflight-configurator/issues/411

It's a temporary solution requiring you to manually install patched up version of BF configurator, next release of official BF configurator is going to fix that.

# NAME

Revolt V1 and V2

## Description

FC designed to run RaceFlight 'closed source' firmware, but has a target called REVOLT in Betaflight.

## MCU, Sensors and Features

### Hardware
  - MCU: STM32F405RGT6
  - IMU: ICM-20602
  - Virtual Com Port (VCP)
  - Blackbox 
  - PPM/UART Shared
  - Battery Voltage Sensor (VBAT)
  - Boot Pads (no button)

### Features

## Manufacturers and Distributors

https://revoltfc.com/index.html

## Designers


## Maintainers

## Similar Targets

This is a variant of the REVO, but is a cut down version, many of the pin outs of the STM MCU are the same.

## FAQ & Known Issues


## Other Resources

