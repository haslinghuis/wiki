# NAME
MATEKSYS F411-MINI

![Matek F411-MINI](http://www.mateksys.com/downloads/FC/MATEK_F411-MINI.jpg)


## Description
F411+MPU6000, w/ BFOSD, No blackbox 


## MCU, Sensors and Features

### Hardware
* MCU: STM32F411CEU6
* IMU: MPU6000(SPI)
* OSD: BetaFlight OSD w/ AT7456E chip
* Compass & Baro: no
* VCP: Yes
* Hardware UARTS: 1, 2
* Blackbox: no
* PPM/UART Shared:  UART2-RX
* Battery Voltage Sensor: Yes 1:10
* Currsnt Sensor: No (FCHUB-A option)
* Integrated Voltage Regulator: 5V/2A
* Brushed Motor Mosfets: No
* Buttons: BOOT button
* 6 PWM / DShot outputs 
* WS2812 Led Strip : Yes
* Beeper : Yes

### Features
* Built in inverter for SBUS input (UART1-RX)
* 6x DShot outputs without conflict
* VCP, UART1, UART2

## Manufacturers and Distributors
* Matek Systems
  * [F411-MINI](http://www.mateksys.com/?portfolio=f411-MINI)

## Designers
Matek Systems www.mateksys.com

## Maintainers
* Hardware: Matek Systems

## FAQ & Known Issues

Setup Guide http://www.mateksys.com/?portfolio=f411-MINI

Matek FC Facebook Group: https://www.facebook.com/groups/1882519175321708/

**Setup for Matek F411 mini for Tricopter** by Flashted

Setup: 2 front motors facing forward and (tail) rear motor / servo facing you.

On the board.

Motor 1 - (rear) to motor output pin S1

Motor 2 (right) to motor output pin S2

Motor 3 (left) to motor output pin S3

Motor 4 motor output pin S4 Free


In the BetaFlight GUI in the Receiver Tab is a selection for how the model is setup.

Betaflight Defaults to AETR1234 This is an accepted standard setup on Taranis.

In the Configuration tab.

feature SERVO_TILT

feature CHANNEL_FOWARDING

Need to be OFF.

Selecting TRICOPTER sets yaw output to servo automaticly.

Set tail servo.

In the CLI

resource motor 6 none. Then the pin will be free.

Pin on board is S6 which B10 is the location of the pin.

resource servo 1 B10.

save

Nothing needs to be changed in the Servos tab for movement.

Do not draw power from the board with a servo especially on a tricopter. 

Positive, black negative are going to a pdb 5v output.

The yellow or white wire, signal wire can be connected to motor pin 5, motor pin 6, 7 or 8, will work.
 
Motor pin 4 will not work for tail servo due to timing issue.

IF you want to disable the tail servo when it`s not armed, go to cli and type:

set tri_unarmed_servo = OFF 

Or if you want it on.

set tri_unarmed_servo = ON

save

Now check if your servo/motor are tilting in the correct direction.
 
If you move your yaw stick to the left, the motor must tilt to the right. If not, there are 2 ways to fix this.

In cli type:

Set yaw_control_direction = -1 

save

If you quickly move your tail to the right  the motor must tilt quickly to the left, and vice versa.

You can just reverse the yaw direction on your Tx, but it is better to do it in the GUI, so the flight controller does not have to process the data.

Set the endpoints in the GUI so you have 40 degrees deflection in both directions, and neutral at as level as you can get it. 

An analog servo works as will a digital one in most cases my directions were not reversed with an analog servo, but may be with a digital.

 Hopes this helps anybody to figure it out, It took much time to do..

Cheers!!!


