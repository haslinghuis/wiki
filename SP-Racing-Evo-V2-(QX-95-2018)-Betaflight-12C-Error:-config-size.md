I'm a novice builder trying to keep my self sane by working on a 210-220 ultra light weight Quad. 

Flysky i6 transmitter, Flysky F8S receiver (PPM,SBUS) although receiver has a iBus option.

Control Board is configured to the best of my knowledge from your forums and YouTube (which are great and make learning interesting). 

Config for Flysky to control-board responds in Betaflight however the device will not arm and the status report in the CLI indicates a Config error which reads: Errors: 1, config size: 2312, max available config: 4096.

I don't know enough about this type of programming to know how to work the problem and am speculating that this is the reason for the device failing to arm, .

Buzzer and Pre arm both light up Arm fails with only CLI errors of arm-switch and the fact that I'm using the USB link.

# Status:
System Uptime: 77 seconds
Voltage: 75 * 0.1V (2S battery - OK)
CPU Clock=72MHz
SD card: Startup failed
Stack size: 2048, Stack address: 0x10002000
I2C Errors: 1, config size: 2312, max available config: 4096
CPU:24%, cycle time: 131, GYRO rate: 7633, RX rate: 50, System rate: 9

Any suggestions would be appreciated. 