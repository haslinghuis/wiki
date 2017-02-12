### Introduction

Starting with v3.1, servo output assignments are deleted from the firmware. Instead, servo outputs must be explicitly assigned by`resource` CLI command. (For details of the `resource` command, please refer to [Betaflight resource remapping](https://github.com/betaflight/betaflight/wiki/Betaflight-resource-remapping).)

---
### General Information

Use `resource list` to view current running assignments.
```
resource list
```

Servos are assigned to board pads/pins by resource CLI command.
```
resource servo n xYY
```
Here, n is a servo number starting from 1, xYY is a MCU pin identifier, for example, A8, c8 and B10.

Notice that the servo numbers are 1 origin.
This is different from servo numbering in the configurator's Servos tab.
Servos you assign here as 1 and 2 corresponds to servos 0 and 1 in the Servos tab.

---
### Examples
Here are some examples of assignments. If you can't find a one that fit your needs, then you have to go read the hard boiled story: [Working on your own](https://github.com/betaflight/betaflight/wiki/_new#working-on-your-own) in the later half of this page.

__Target/board maintainers, please add example entries that reflect mappings based on the v3.0.1 `pwm_mapping.c`; it can cover all the mappings, not only for servo tilts__

---
#### Example 1: NAZE32 "__Shift by 2__" style assignment
If you have a NAZE32 already setup based on "__Shift Motor Outputs by 2__" rule, and it was working prior to v3.1, here is your assignment.
```
resource motor 1 none
resource motor 2 none
resource servo 1 a8
resource servo 2 a11
resource motor 1 b6
resource motor 2 b7
resource motor 3 b8
resource motor 4 b9
save
```

---
#### Example 2: SPRacing F3 Controlling External PWM triggered Buzzer

*This example is for controlling PWM triggered buzzer on Matek 5-in-1 PDB, but can be applied to other cases.*

Objective: To control an external PWM controlled device (Matek 5-in-1 PDB buzzer), with SPRacing F3, using `IO_1[4]` (RC CH2) with AUX2 switch.

This can be accomplished by one of two ways:

> A. (Available for all v3.1) Use SERVO_TILT and assign AUX channel of your choice to servo 0 on the Servo tab.
> 
> B. (Limited to v3.1.5 and later) Use CHANNEL_FORWARDING and set CLI variable channel_forwarding_start to your switch channel number.

Option A is recommended if gimbal servo is not used and number of devices are less than or equal to two
(SERVO_TILT can only control two channels).

Here's step by step for option A, controlling `IO_1[4] (RC CH2)` with AUX2:

(1) BF3.1 does not configure any servo for you; you have to assign it explicitly.

(1-1) Make sure you are not using PPM input (CH2 shares a timer with PPM).

(1-2) Use following CLI command
```
resource pwm 2 none
resource servo 1 a1
save
```

(2) Turn on `SERVO_TILT`
(And turn off `CHANNEL_FORWARDING` if you have it on.)

(3) On Servo tab, check `A2` on `servo 0`.
Resource command above specified `servo 1`, but this number is 1-origin, servo tab servos are 0-origin, so these are the same.

(4) At this point, you should be able to confirm `servo 0` on Motor tab responding to AUX2 switch inputs.

(5) And if you connect your external device to `IO_1[4]`, you should be able to confirm it is working.

---
#### Example 3: SPRacing F3 EVO Controlling External Still Camera Shutter

Motor 8 was used
```
resource servo 0 b1
```

---
#### New Example Place Holder

---
### Working on your own

There are several rules for successful servo MCU pin assignment.

1. **The MCU pin must be connected to an accessible board pad or through-hole**  
There may be a free unused MCU pin on your board, but without the pin broken out as an accessible pad or through-hole, you will end up with horrendous work of breaking out MCU pins by micro soldering. 

2. **The MCU pin must have a timer**  
MCU pins are associated with different functions. Some are capable of working as UART signal pins, some are capable of working as I2C or SPI bus signal pins, and yet some are only capable of driving the pin logic high or low. Servo outputs need the timer function.  
Pins associated with timer can be found out by executing `resource list` command and looking for "MOTOR" or "PWM" entries.

3. **The timer should not collide with other uses**  
This rule requires a bit of explanation. An MCU pin associated with _timer_ is connected to a _timer channel_ of the _timer_. Each _timer_ has it's own _time base_ (or clock), and all _timer channels_ on the same _timer_ behaves according to the _time base_.   
 Think, for example, a group of people looking at a (physical) clock with only one hand. Each is given a task of yelling out loud when the second hand advanced certain number of times. If you give them a clock with a hand advancing every second, they will yell based on seconds.
Now think of another group of people and give them a clock with a hand that advances once every hour. They will be yelling based on hours.
You can NOT mix someone told to yell at every X seconds with someone told to yell at every Y hours in a same group.

(To continue...)