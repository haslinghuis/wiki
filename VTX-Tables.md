## VTX Tables for Use in Configurator

(Download the .json file, then use 'Load from file' in the 'Video Transmitter' tab to load it, **then click 'Save'** to save the VTX table on the flight controller.)

|Region|Files|
|-|-|
|USA|[IRC Tramp](resources/vtx_tables/vtx_table_irc_tramp_us.json)|
| |[SmartAudio 1.0](resources/vtx_tables/vtx_table_smart_audio_1_0_us.json)|
| |[SmartAudio 2.0](resources/vtx_tables/vtx_table_smart_audio_2_0_us.json)|
| |[SmartAudio 2.1](resources/vtx_tables/vtx_table_smart_audio_2_1_us.json)|
| |[RTC6705 (on board)](resources/vtx_tables/vtx_table_rtc6705_us.json)|
|EU|[IRC Tramp](resources/vtx_tables/vtx_table_irc_tramp_eu.json)|
| |[SmartAudio 1.0](resources/vtx_tables/vtx_table_smart_audio_1_0_eu.json)|
| |[SmartAudio 2.0](resources/vtx_tables/vtx_table_smart_audio_2_0_eu.json)|
| |[SmartAudio 2.1](resources/vtx_tables/vtx_table_smart_audio_2_1_eu.json)|
| |[RTC6705 (on board)](resources/vtx_tables/vtx_table_rtc6705_eu.json)|


## Setting up VTX Tables in CLI

See https://github.com/betaflight/betaflight/blob/master/docs/VTX.md#vtx-table


## VTX Button usage
	
While the VTX button is held the STATUS 2 LED will flash N times per second indicating the action that will be taken when the button is released. The flashing starts as soon as the button is held. e.g. You press the button, count flashes and then release as appropriate.
While the VTX button is held the STATUS 2 LED will flash N times per second indicating the action that will be taken when
the button is released. The flashing starts as soon as the button is held. e.g. You press the button, count flashes and
then release as appropriate.
	
	| Duration      | Function                  | Flashes   |
	|---------------|---------------------------|-----------|

	
Example to cycle VTX power

```
| 0 seconds     | 1 second     | 2 seconds    | 3 seconds    | 4 seconds    | 5 seconds    | 6 seconds or more|
[-HOLD BUTTON------------------------------|-RELEASE BUTTON-NOW------------|-RELEASED TO LATE TO CHANGE POWER |
| 4 Flashes     | 3 flashes    | 3 flashes   | 2 flashes    | 2 flashes    | 1 flash      | 1 flash           |
| 0 seconds     | 1 second     | 2 seconds   | 3 seconds    | 4 seconds    | 5 seconds    | 6 seconds or more |
|-HOLD BUTTON------------------------------|-RELEASE BUTTON-NOW------------|-RELEASED TOO LATE TO CHANGE POWER|
| 4 Flashes     | 3 flashes    | 3 flashes   | 2 flashes    | 2 flashes    | 1 flash      | 1 flash           |
```
	
The VTX button works with ALL VTX systems including onboard RTC6705, Tramp and SmartAudio.
	
	
If the VTX can be turned off then POWER 0 will turn off the VTX and POWER 1 will set the VTX into it's lowest power output.
If the VTX cannot be turned off then POWER 0 will set the VTX into it's lowest power output.


Some videotransmitters have restrictions on its usage. For example, SmartAudio V1.0 and V2.0 devices can only enter pitmode on power-up.
Betaflight can make the these devices leave pitmode, but not enter it.