## Setup

- Wiring  
Just wire the Tramp telemetry wire to a free hardware UART (TX) port.

- Configuration  
The up to date configurator supports easy configuration of the SmartAudio on the selected port.

1. Goto Ports tab
2. Select IRC Tramp from Peripherals drop down menu
3. Speed can be left at AUTO.

![Select IRC Tramp from peripherals](https://cloud.githubusercontent.com/assets/14850998/22005847/ddc6641a-dca9-11e6-8de3-64dc39ecb5cf.png)


## IRC Tramp CMS guide
The top menu for IRC Tramp VTX looks like this.
![IRC Tramp CMS menu](https://cloud.githubusercontent.com/assets/14850998/21991074/8bd7c464-dc54-11e6-822c-53defecdc915.jpg)
While most of the entries are intuitive, there are several things that need additional explanation.

### Status Line
The status line on the top menu page of Tramp VTX menu indicates current status of the vtx in the following format:

```
m bc ffff ppp
```

where

`b` : Current transmitting band, `A` (BOSCAM A), `B` (BOSCAM B), `E` (BOSCAM E), `F` (FatShark/NexWave) or 'R' (Raceband).

`c` : Current transmitting channel, `1` through `8`.

`ffff`: Current transmitting frequency.

`t`: If thermal protection is in effect, this field is '`*`', otherwise space ('` `').

`ppp`: Current transmitting RF power, numeric value in mW (milli-Watt).

Note that the status line indicates "running" status of the VTX device, and values may be different from band, channel and power setting entries below the status line.

### Thermal Protection
When the thermal protection is in effect, the device will automatically regulate the RF power. Therefore, value set by `POWER` entry will not be displayed on the status line.