Not for public viewing yet.

List of compatible pins.

---
OMNIBUS(F3)

| Pin | Label     | S.Audio | Tramp | S.Port | Note                    |
|-----|-----------|---------|-------|--------|-------------------------|
| A8  | LED strip | NG      | ?     | ?      |                         |
| B4  | PPM (*1)  | OK      | ?     | ?      | When PPM not in use     |
| B6  | PWM8/SCL  | OK      | ?     | ?      | I2C must be de-configured? Need furthe testing. |
| B7  | PWM7/SDA  | OK      | ?     | ?      | Ditto                        |

---
OMNIBUSF4

| Pin | Label     | S.Audio | Tramp | S.Port | Note |
|-----|-----------|---------|-------|--------|------|
| A1  | PWM5      | OK      | ?     | OK     |      |
| A8  | PWM6      | OK      | ?     | ?      |      |
| B14 | PPM       | OK      | ?     | ?      |      |
| B15 | CH2       | OK      | ?     | ?      |      |
| C8  | CH5       | OK      | ?     | ?      |      |
| C9  | CH6       | OK      | ?     | ?      |      |

---
SPRACINGF3

| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| A0  | IO_1[3] PPM/CH1   | OK      | ?     | ?      | When PPM is not in use |
| A1  | IO_1[4] CH2       | OK      | ?     | ?      | When PPM is not in use |
| B4  | IO_1[5] CH5       | OK      | ?     | ?      |                        |
| B5  | IO_1[6] CH6       | OK      | ?     | ?      |                        |
| A8  | IO_1[7] LED strip | OK      | ?     | ?      |                        |
| B0  | IO_2[5] CH7       | OK      | ?     | ?      |                        |
| B1  | IO_2[6] CH8       | OK      | ?     | ?      |                        |
| B8  | M5                | NG      | ?     | ?      | TIM4 crash with M3&M4  |
| B9  | M6                | NG      | ?     | ?      | TIM4 crash with M3&M4  |
| A2  | M7                | OK      | ?     | ?      |                        |
| A3  | M8                | OK      | ?     | ?      |                        |

---
SPRACINGF3EVO

| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| A8  | LED strip         | ?       | NG    | ?      |                        |
| B1  | M8                | ?       | OK    | ?      |                        |