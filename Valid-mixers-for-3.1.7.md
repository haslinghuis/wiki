### Valid mixers on 3.1.7

- Some rarely used mixers are not supported (dropped) from F1 and F3 firmwares due to flash space limitation.
- Some mixers are not supported from the beginning (or at least I couldn't find any remains of the code).
- If unsupported mixer is specified, it will reset to custom mix (on 3.1.8 or 3.2 and later).


| Mixer            | F1 (*1) | F3 | F4 & F7 | Note |
|------------------|---------|----|---------|------|
| QUADX            | o       | o  | o       |      |
| QUADX 1234       | o       | o  | o       |      |
| QUAD+            | o       | o  | o       |      |
| Tricopter        | o       | o  | o       |      |
| Gimbal           | o       | o  | o       |      |
| Hex +            | x       | x  | o       |      |
| Hex X            | o       | o  | o       |      |
| Hex H            | x       | x  | o       |      |
| Octo Flat +      | x       | x  | o       |      |
| Octo Flat X      | x       | x  | o       |      |
| Flying Wing      | o       | o  | o       |      |
| Airplane         | o       | o  | o       |      |
| Heli 120         | x       | x  | x       | No code      |
| Heli 90          | x       | x  | x       | No code     |
| Single Copter    | o       | o  | o       | Requires custom mmix      |
| Dual Copter      | x       | x  | o       |      |
| Bicopter         | x       | x  | o       |      |
| V-tail Quad      | o       | o  | o       |      |
| A-tail Quad      | o       | o  | o       |      |
| Y4               | o       | o  | o       |      |
| Y6               | x       | x  | o       |      |
| Octo X8          | x       | x  | o       | No code, can be emulated by mmix      |
| PPM to SERVO     | x       | x  | x       | No code, can be emulated by smix      |
| Custom           | o       | o  | o       |      |
| Custom Airplane  | o       | o  | o       |      |
| Custom Tricopter | o       | o  | o       |      |

*1: CJMCU and MICROSCISKY only supports QUAD mixer.