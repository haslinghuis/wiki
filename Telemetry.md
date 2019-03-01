Telemetry is information sent back to your RC transmitter via the RC data link.  For example, telemetry allows for your RC transmitter to read out your main battery voltage or RSSI.  For telemetry to work your RC receiver and transmitter must support it.  The specific data that Betaflight will send via telemetry depends on the telemetry protocol being used. For example, FRSky Smartport will send a certain set of information while Crossfire will send another set.  

Here is the set of telemetry fields send via the Crossfire protocol.
https://github.com/betaflight/betaflight/blob/daa6df80248c9a806b7d77c402d415a15f4e2667/src/main/telemetry/crsf.c#L168
int32_t     Latitude ( degree / 10`000`000 )
int32_t     Longitude (degree / 10`000`000 )
uint16_t    Groundspeed ( km/h / 10 )
uint16_t    GPS heading ( degree / 100 )
uint16      Altitude ( meter ­1000m offset )
uint8_t     Satellites in use ( counter )

Here is the set of telemtry fields sent via Smartport can be seen here : https://github.com/betaflight/betaflight/blob/daa6df80248c9a806b7d77c402d415a15f4e2667/src/main/telemetry/smartport.c#L89

