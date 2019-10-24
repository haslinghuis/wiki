# Unified Targets for users
* Determine the target to use. 

Yes the chip says STM32F405 on it, but that does not describe the board.
* Choose your target
```
MATEKF411 (MTKS)  <-- Unified target
MATEKF411 (Legacy)
```
FAQ: What do these four letters mean? They refer to the manufacturer of the board. A list is available in  [Manufacterers.md](https://github.com/betaflight/unified-targets/blob/master/Manufacturers.md)

Tip: remember to save a backup of your config, like as a `diff`, *before* you flash a new version of betaflight.

* `Load Firmware [Online]` then `Flash Firmware`

* Connect to configurator, click on `Apply Custom Defaults` when prompted:
![Picture of a notice that asks the user to apply custom defaults](https://raw.githubusercontent.com/wiki/betaflight/betaflight/images/betaflight/apply_custom_defaults_prompt.png)

If you are having an issue with the unified target, try the legacy target for now, and file an issue on the [Issue Tracker](https://github.com/betaflight/betaflight/issues) if the unified target is missing anything that it should have. 

Todo: which target for example?, also needs some images.

# Tips for working with Unified targets
## Working on firmware
Save a copy of the unified target to your computer from [the repository](https://github.com/betaflight/unified-targets/tree/master/configs/default), lets use `MTKS-MATEKF411.config` as an example.

Open up the file in a text editor and take a look at the first line.

> \# Betaflight / **STM32F411** (S411) 4.1.0 Jun 25 2019 / 10:27:57 (2a6e94d03) MSP API: 1.42

In this case, `STM32F411` is the processor target used, so when you compile a target you'll want to use `make TARGET=STM32F411`

### Combine and flash with the configurator
In the configurator load the `.config` file first, and then load the `betaflight_4.x.x_STM32F411.hex`, now flash the firmware. On the first connect you will be prompted to `Apply Custom Defaults` just like the regular flashing procedure

### make_config_hex.sh

`make_config_hex.sh` is a script in `src/utils` that can be used to combine a unified target configuration with a firmware image. The combined .hex may be useful to share with other users of the same flight controller. Users of the combined .hex will be prompted to `Apply Custom Defaults`, just like the regular flashing procedure.

The `srec_cat` program is part of [srecord](http://srecord.sourceforge.net/), which is available in ubuntu under universe. `apt install srecord`

Windows binaries to not seem available, but they do have [instructions](http://srecord.sourceforge.net/windows.html)

Take a look at [src/utils/make_config_hex.sh](https://github.com/betaflight/betaflight/blob/master/src/utils/make_config_hex.sh) for more information.
