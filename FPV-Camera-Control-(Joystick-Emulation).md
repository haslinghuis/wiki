## Description
This functionality provides OSD joystick emulation for HS1177-style cameras, e.g. cameras having a single OSD input pin driven by a button resistor divider.
Most cameras up to date seem to adhere to the design and have the following resistor nominals:

* 47 kΩ - camera internal resistor
* 45 kΩ - enter
* 27 kΩ - left
* 15 kΩ - up
* 6.8 kΩ - right
* 0 - down

## Setting up
* `resource camera_control PIN` -  allows you to designate a `PIN` for camera control functionality. The list of suitable pins depends on the way you're going to wire your camera to the FC.
* `set camera_control_mode = hardware_pwm` - mode of operation, `software_pwm` is the least restrictive in terms of available PIN selection, but it requires both a resistor and a capacitor to work properly; `hardware_pwm` is almost guaranteed to work with just a resistor given you can spare a timer for it; `dac` (not yet implemented) is supported on the very few FCs that have a DAC pin broken out and unoccupied by other functions, it works by direct connection to the camera.
* `set camera_control_ref_voltage = 330` - voltage (in 10 mV steps) measured across your camera's floating `OSD` and `GND` pins, usually 3V3, but not guaranteed, e.g. my RunCam Sky has 3V4, and some cameras have reportedly have as low as 3V1.
* `set camera_control_key_delay = 180` - the duration of each key press (in ms presence at the `camera_control` pin, after consulting with RunCam it was set to 180 ms to accommodate most cameras, while some of them accept as low as 125 ms.

## Modes of operation
### Hardware PWM
Requires a 150-600 Ω resistor inline from your FC `PIN` to Camera `OSD`. Additional `GND` connection is advised.
#### How do I select a suitable `PIN`?
@todo

### Software PWM
Requires a 150-600 Ω resistor and a 1-10 µF capacitor, forming an RC-filter. Resistor goes from `PIN` to `OSD`, while the capacitor should be connected between `OSD` and `GND`.
@todo draw a schematic

### DAC
Requires no additional components, wire your `PIN` to `OSD`, preferably with a `GND` connection as well.

## Accessibility
* RC Stick Commands
* `MSP_CAMERA_CONTROL`
* MSP over Telemetry, e.g. Lua scripts for FrSky radios ([Pull Request #23](https://github.com/betaflight/betaflight-tx-lua-scripts/pull/23))

## Stick Commands
![Camera Control Stick Commands](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/camera-control-stick-commands.png)

## Example FC configurations

Emax Magnum stack + Foxeer Arrow v3.
600ohm resistor in line
Camera control Mapped to Motor 5 output
set camera_control_key_delay = 125 (This made it work more reliably for me)