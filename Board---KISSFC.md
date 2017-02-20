# KISSFC

_Keep it simple stupid._

## Description

KISSFC is a STM32F3 based flight controller with integrated voltage regulator, brushed motor drivers and a shared PPM/UART RX input pin.

## MCU, Sensors and Features

### Hardware

  - MCU: STM32F3
  - IMU: MPU6050
  - IMU Interrupt: Yes
  - BARO: No
  - VCP: Yes
  - Hardware UARTS: 3
  - OSD: No
  - Blackbox: No
  - PPM/UART Shared: UART2
  - Battery Voltage Sensor: Yes, directly connected, no wiring necessary
  - Integrated Voltage Regulator: Yes, unknown voltage limit
  - Brushed Motor Mosfets: Yes
  - Buttons: 1 - DFU

### Features
  - Current Sensor: Yes (as of BF 3.1.6)
  - BlHeli passthrough: No (due to buffered outputs)
  - WS2811 Led Strip: Yes (as of BF 3.1)
  - Transponder: No

### KISS FC Betaflight additional features pad assigments

PAD ON KISS FC|FUNCTION
---|--------
PITCH|Current sensor
PWM5 (Motor 5)|LED strip
AUX1|Softserial 1
ROLL|Softserial 2

Softserial pad assignment can be changed with the `resource` command. Current sensor and LED strip assignments are hard-coded and can not be changed runtime at the moment.

## Hardware Designs (if available)

## Manufacturers and Distributors

[Flyduino.net](https://flyduino.net)

Available here: http://flyduino.net/KISS-FC-32bit-Flight-Controller-V103_1

## Designers
_(add your name here if you conributed to the design of this board)_

[fedorcomander](https://github.com/fedorcomander)

## Maintainers
_(add your name here if you help test or contribute code for this board)_

## Similar Targets
_(add links board descriptions here that are similar in features or function, but have a separate target)_

## Variants
_(add links to boards here that are similar in features or function, but use this target when flashing)_

## FAQ & Known Issues
_(add FAQs, known issues and workarounds specifically related to this board. please link work in progress issues to the related github issue or pull request)_

###Remapping Motor pins with the Resource command by NarcolepticLTD:   
I just swapped over from kiss firmware to BF 3.1.1 to give the new features a try (and didn't want to resolder anything). I noted in previous releases that the motor outputs had been updated to match in this target, but not sure if that carried over to 3.1.1 - anyways, for anyone curious on how to do this, I simply held down the boot loader when plugging in, released, and then flashed 3.1.1 (kiss fc target), and had no issues with the install.

Once it's installed, if like me you're wired for kiss firmware and don't want to resolder/mess about, you can use the new resource remapping tools to fix the motor order. Pick quadX1234 for your mixer, and change your motor mapping in the CLI from this:  

`resource MOTOR 1 B14`  
`resource MOTOR 2 B00`  
`resource MOTOR 3 B15`  
`resource MOTOR 4 A08`  

To this:  
`resource MOTOR 1 A08`  
`resource MOTOR 2 B00`  
`resource MOTOR 3 B14`  
`resource MOTOR 4 B15`  

once I had that set everything checked out in the motors tab and typical props off benchtests... hover test completed. Hopefully I'll get a chance to get out and fly in the next few days and see how the new anti_gravity settings sort my pitch issues (sick of doing wheelies).

Quick edit to note that I haven't yet bothered to see if telemetry will need to be remapped - I may mess with it later. 

## Other Resources

Pinouts, schematics and RX wiring: http://nathan.vertile.com/blog/2016/07/29/betaflight-kiss-flight-controller/#pinout

Rcgroups Thread: http://www.rcgroups.com/forums/showthread.php?t=2555204

## Image

![](http://flyduino.net/bilder/produkte/gross/KISS-FC-32bit-Flight-Controller-V103.jpg)