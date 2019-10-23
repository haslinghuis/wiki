# Unified Targets for users

Choose your target
```
MATEKF411 (MTKS)  <-- Unified target
MATEKF411 (Legacy)
```
FAQ: What do these four letters mean? 

You could look up AIRB, MTKS, etc on [Manufacterers.md](https://github.com/betaflight/unified-targets/blob/master/Manufacturers.md)

Suggesting Full Chip Erase is good idea when switching between versions, especially from like 4.0.6 to 4.1.0. 
`Load Firmare [Online]` then `Flash Firmware`

Connect to configurator, click on `Apply Custom Defaults` when prompted

Todo: which target for example?, also needs some images.

# Tips for working with Unified targets
## Working on firmware
Save a copy of the unified target to your computer from [the repository](https://github.com/betaflight/unified-targets/tree/master/configs/default), lets use `MTKS-MATEKF411.config` as an example.

Open up the file in a text editor and take a look at the first line.

> \# Betaflight / **STM32F411** (S411) 4.1.0 Jun 25 2019 / 10:27:57 (2a6e94d03) MSP API: 1.42

In this case, `STM32F411` is the processor target used, so when you compile a target you'll want to use `make TARGET=STM32F411`

In the configurator load the `.config` file first, and then load the `betaflight_4.x.x_STM32F411.hex`, now flash the firmware. On the first connect you will be prompted to `Apply Custom Defaults` just like the regular flashing procedure