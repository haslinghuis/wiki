# Motivation

FPort is a new RC protocol that was developed by FrSky in collaboration with Betaflight. It has a number of improvements over existing protocols used by FrSky hardware:

- only one serial connection is required, RC control information, telemetry (including MSP tunneling), and RSSI are all sent over this connection;
- the serial connection uses 115200, N81, making it easier to implement on hardware that has limited support for 'exotic' modes;
- it is uninverted (not possible with the test firmware for existing RX).

A driver for FPort has been added to Betaflight. It is available in nightly releases, and will be included in Betaflight 3.3.

The detailed specification for FPort is available here: [1].




# Testing FPort

## Requirements:
- a FrSky XSR or X4R(SB) receiver;
- a free hardware port on the flight controller (F3 or better) that is capable of running SmartPort (i.e. must be able to run inverted bidirectional).

## Installation
1. Download and install the XSR / X4R(SB) firmware: [2]. Instructions for the firmware installation can be found at [3], [4];
2. Install a Betaflight nightly build (#366 or newer required) from [5] onto your flight controller;
3. Connect the SmartPort port on your receiver to the inverted bidirectional port on your flight controller. On F3 / F7, the receiver is connected to the TX pin of the serial port, on F4 the connection will be dependent on how the bidirectional inverter is designed - consult your flight controller manual; (Effectively, this connection uses the same pins on both sides that would be used to connect SmartPort if a non-FPort firmware was used.)
4. Configure your flight controller. Enable 'serial RX' for the port the receiver is connected to, choose 'Serial Rx' as receiver type, and 'FPort' as protocol. For F3, set `serialrx_halfduplex = on` in CLI. After all is done, the relevant bits of a `dump` should look like this (assuming we're using UART3):

F3 / F7:

    serial 2 64 115200 57600 0 115200
    set serialrx_provider = FPORT
    set serialrx_halfduplex = ON

F4:

    serial 2 64 115200 57600 0 115200
    set serialrx_provider = FPORT
    set serialrx_halfduplex = OFF


5. Bind your receiver to your transmitter;
6. Test RC control: With the transmitter on and the flight controller connected to the configurator, make sure the bars in the 'Receiver' tab move when you move the sticks on the transmitter;
7. Test telemetry: (In OpenTx, a rescan of the sensors is required, since the sensor ids are different between SmartPort and FPort.) Check that the telemetry screen shows the values from your flight controller. (caveat: With FPort it is possible that RC commands work, but telemetry doesn't. If this happens, it means that the serial connection that you are using is not bidirectional, and the receiver => flight controller data flow works, but flight controller => receiver doesn't. Id this happens, check your port settings, and (for F4) make sure that the port you are using can support inverted bidirectional.)
8. If you want to use the Betaflight lua telemetry scripts on Taranis / Horus, download and install the latest release (1.0 or newer) from [6].
9. Happy flying. ;-)

(As always, any sort of feedback and bug reports are appreciated, please drop them here: [7])





[1] (release by FrSky pending)

[2] https://github.com/betaflight/betaflight/files/1484373/XSR_FPport_Betaflight_171114.zip

[3] https://oscarliang.com/flash-frsky-rx-firmware/

[4] http://thrustworx.com/frsky-x-series-receiver-sensor-s-port-firmware-flashing-9xr-pro-complete-guide/

[5] https://ci.betaflight.tech/job/Betaflight/

[6] https://github.com/betaflight/betaflight-tx-lua-scripts/releases

[7] https://github.com/betaflight/betaflight/issues