In Betaflight 3.3.0 there will be a possiblity to get a Received Signal Strength Indicater (RSSI) in OSD in a couple of different ways. 

* True RSSI as reported by SPM4649T receivers. Preconditions:
  * Rx is connected and setup for Telemetry, as described here: https://github.com/betaflight/betaflight/wiki/Spektrum-SPM4649T-SRXL-Telemetry-setup.
  * Rx firmware is updated to at least version 1.1.9
* Fake RSSI based on radio link fades reported by any Spektrum Satellite Rx. No preconditions.

All you have to do is to 
* Select any free RC channel for RSSI in the BFC Receiver Tab. AUX8 for example.
* Enable RSSI in the OSD tab of BFC.

Done.
