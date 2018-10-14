## Overview

- _**Pinio**_ is an abstraction of simple GPIO (General Purpose I/O) pin facility, and _**piniobox**_ is a facility to associate boxes (modes) to `pinio`.
- Most targets are build to be capable of configuring up to four pins for the pinio facility, which can then be controlled by the piniobox facility.

## Pinio

An MCU pin can be assigned to `pinio` by `resource` CLI command.
```
resource pinio <index> <pinid>
```
Example of `resource pinio`
```
resource pinio 1 A1
resource pinio 2 A8
resource pinio 3 C9
resource pinio 4 D2
```

I/O configuration of each pin is specified by `pinio_config` CLI variable, which is a comma separated list of 8-bit values. MSB represents inversion, and remaining 7-bits specify I/O mode as defined in drivers/pinio.h (Only push-pull output is defined for this PR).
```
#define PINIO_CONFIG_OUT_INVERTED 0x80
#define PINIO_CONFIG_MODE_MASK    0x7F
#define PINIO_CONFIG_MODE_OUT_PP  0x01
```

Example of `pinio_config`
```
set pinio_config = 1,129,1,1
```
sets Output-Push-Pull for `pinio` 1, 3 and 4, and Inverted Output-Push-Pull for `pinio` 2. Default value is 1 (Output-Push-Pull).

## PinioBox

CLI variable `pin_iobox`, comma separated list of permanent ID of boxes, associate the boxes to corresponding `pinio`s. Once associated, the boxes's activation status are reflected to the associated `pinio`s (and then to pins). Since the piniobox facility has it's own capability of monitoring activation state of boxes, it operates independently from what boxes are meant for. (In other words, the piniobox _**adds**_ pinio capability to boxes.)

For permanent ID of boxes, refer to `interfaces/msp_box.c`.
Permanent IDs 40 through 43 are user defined boxes, which are activated and appear as `USER1` through `USER4` in the list of boxes at places such as Modes tab in configurator.

Example of `pinio_box`
```
set pinio_box = 0, 39, 43, -1
```
With this assignment, `pinio` 1 through 4 are associated with boxes as follow.

| `pinio` | Box | Permanent ID |
|----|----|----|
|`1` | `ARM` | `0` |
|`2` | `VTX PIT MODE` | `39` |
|`3` | `USER4` | `43` |
|`4` | Unused | `-1` |

## FAQ
- None yet.