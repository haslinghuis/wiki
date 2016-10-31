# **Overview**

The FuryF7 is the next iteration in the Fury line of flight controllers.  Designed around the STM32F7 MCU, it features a simple layout that makes connecting components convenient and easy to accomplish.  The FuryF7 utilizes an SPI gyro (ICM-20689) for super fast communication with the MCU.  Flight logging can be easily done by writing to the onboard data flash or SDCard slot.  

**RCGroups Thread:  https://www.rcgroups.com/forums/showthread.php?t=2628430**

# **Board Features**

* F7 processor (allows you to run high loop times and gives you 3x dedicated UART outputs
for things such as GPS, OSDs, Telemetry, etc.)
* ICM20689 gyro - The ICM20689 is the new gyro designed by Invensense to replace the MPU-6000.
* Large Solder Pads on edge of board
* USB connector
* Spektrum Receiver connector port (additional connector is recommended but not required
if using Spektrum Hardware.
* 3v3 output for I2C connections
* Buzzer connection availability
* Led connection availability
* Current sensor connections available

# **Board Specifications:**
* 5V input or On-board 5v, 2A Switching Regulator
* Standard 36x36 Board (30.5x30.5 mounting)
* STM32F7: 32-Bit, 216MHz, 1MB Processor (floating point arithmetic, lots of I/O)
* 3 hardware serial ports.
* USB VCP (can be used at the same time as the serial ports).
* 4 PWM outputs (dedicated for quads).
* 3.3v regulator output for external devices/Spektrum (up to 500mA)
* Dedicated PPM/SerialRX input header pins.
* Dedicated SPEKTRUM adapter port.
* Dedicated I2C headers.
* ICM20689 Mems Gyro/Accelerometer (The ICM20689 is less sensitive to noise then the 9250
or 6500. There is no need to soft mount the controller)
* SPI Gyro connection
* Optional SPI MS5611 Barometer on bottom of board for easy foam covering isolation.
* On-Board MicroSD Card Support for blackbox data logging (no fuss easy Data logging so that you can get the perfect tune.
* On-Board 16MB Flash for blackbox data logging
* Voltage monitoring (built in Voltage divider)
* Current monitoring (with external current sensor)
* RSSI monitoring (if your receiver of choice has an output)
* Buzzer Connector
* LED Strip Connector
* SWD Port
* Thoughtful, easy-to-build layout
* Edge launch pins for a low profile build, also better for direct soldering.

# **Board Layout**

![](http://i.imgur.com/vfF04cL.jpg)

![](http://i.imgur.com/bw0L0R8.jpg)