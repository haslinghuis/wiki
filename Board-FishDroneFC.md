# FishDrone FPV Flight Controller

### Description
The FishDroneFC F4 flight controller is an integrated flight controller and OSD and VTX specifically designed for ease of use and outstanding flight performance. The SPI-connected ICM20608 inertial motion sensor was chosen for it's high reliability, accuracy and update speed. This board has no problem running fast loop times and ESC protocols. There is an onboard barometer for altitude sensing along with an On Screen Display (OSD) chip directly connected to the main processor (MCU). This tight integration between the MCU and the OSD enables fast updates to the display and easy configuration of the OSD, which is managed straight from the BetaFlight configuration tool. You no longer need to worry about the extra hassle of configuring your OSD with a USB/UART adapter and 3rd party configuration tool, it's all built into the flight control software.

### Hardware
- MCU : STM32F405RGT6
- IMU : ICM-20608-G (SPI)
- IMU Interrupt : Yes
- Compass & Brao : no support it (only  design for fpv)
- VCP : Yes
- OSD : Yes, BetaFlight OSD (BFOSD)
- VTX : Yes ( RTC6705 chip )
- Blackbox : Yes ( 16mb Flash )
- Receiver : PPM / RX bus
- OutPut : 4 esc ( support oneshot,mutishot,dshot)
- Battery Voltage Sensor: Yes, directly connected, no wiring necessary
- Integrated Voltage Regulator: Yes, support for 2S-6S battery
- Buttons : 1 - DFU
- Brushed Motor Mosfets : No
- UART : UART3 for smartport & UART6 for sbus and more rxBus receiver.

### Features
- All in one design
- Current Sensor : Not implemented
- BlHeli passthrough : Yes
- WS2811 Led Strip : Yes
- Beeper : Yes
- Transponder: No

### Manufacturers and Distributors

### Hardware Designs 
The hardware is currently closed source. It may be in the future that older revisions will be made publicly available.

### Other Resources
Rcgroups Thread : 

### FAQ 

### Images
