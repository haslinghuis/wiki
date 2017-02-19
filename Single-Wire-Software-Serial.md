### Software Serial List of compatible pins

Not for public viewing yet.

List of compatible pins.

NOTE:
- The OK means it is compatible, and does not warrant it will work when it is configured as a part of a complete system.

---
OMNIBUS(F3) (by @jflyper)

| Pin | Label     | S.Audio | Tramp | S.Port | Note                    |
|-----|-----------|---------|-------|--------|-------------------------|
| A8  | LED strip | NG      | NG    | NG     |                         |
| B4  | PPM (*1)  | OK      | ?     | OK     | When PPM not in use     |
| B6  | PWM8/SCL  | OK      | OK    | OK     | I2C must be de-configured? Need furthe testing. |
| B7  | PWM7/SDA  | OK      | ?     | ?      | Ditto                        |

---
OMNIBUSF4 (by @jflyper)

| Pin | Label     | S.Audio | Tramp | S.Port | Note |
|-----|-----------|---------|-------|--------|------|
| A1  | PWM5      | OK      | ?     | OK     |      |
| A8  | PWM6      | OK      | ?     | ?      |      |
| B14 | PPM       | OK      | ?     | ?      |      |
| B15 | CH2       | OK      | ?     | ?      |      |
| C8  | CH5       | OK      | ?     | ?      |      |
| C9  | CH6       | OK      | ?     | ?      |      |

---
OMNIBUSF4SD (by @jflyper)

| Pin | Label     | S.Audio | Tramp | S.Port | Note |
|-----|-----------|---------|-------|--------|------|
| A1  | PWM5      | ?       | ?     | ?      |      |
| A8  | PWM6      | OK      | ?     | OK     |      |
| B14 | PPM       | ?       | ?     | ?      |      |
| B15 | CH2       | ?       | ?     | ?      |      |
| C8  | CH5       | ?       | ?     | ?      |      |
| C9  | CH6       | ?       | ?     | ?      |      |

---
SPRACINGF3 (by @jflyper)

| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| A0  | IO_1[3] PPM/CH1   | OK      | ?     | ?      | When PPM is not in use |
| A1  | IO_1[4] CH2       | OK      | ?     | ?      | When PPM is not in use |
| B4  | IO_1[5] CH5       | OK      | ?     | ?      |                        |
| B5  | IO_1[6] CH6       | OK      | ?     | ?      |                        |
| A8  | IO_1[7] LED strip | OK      | ?     | ?      |                        |
| B0  | IO_2[5] CH7       | OK      | ?     | ?      |                        |
| B1  | IO_2[6] CH8       | OK      | ?     | ?      |                        |
| B8  | M5                | NG      | ?     | NG     | TIM4 crash with M3&M4  |
| B9  | M6                | NG      | ?     | ?      | TIM4 crash with M3&M4  |
| A2  | M7                | OK      | ?     | ?      |                        |
| A3  | M8                | OK      | ?     | ?      |                        |

---
SPRACINGF3EVO

| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| A8  | LED strip         | ?       | NG    | ?      | @pafleraf              |
| B1  | M8                | ?       | OK    | OK     | @pafleraf              |

---
REVOLT

| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| B6  | LED               | ?       | ?     | NG     | @alenl2; Need further testing            |
| C1  | CRNT              | ?       | ?     | NG     | @alenl2; No timer?     |
| A9  | TX1               | ?       | ?     | NG     | @alenl2; Timer conflict? (need checking) |
| A10 | RX1               | ?       | ?     | NG     | @alenl2; Timer conflict? (need checking) |
| B10 | TX3               | ?       | ?     | NG     | @alenl2; Timer conflict? (need checking) |
| B11 | RX3               | ?       | ?     | NG     | @alenl2; Timer conflict? (need checking) |
| C6  | TX6               | ?       | ?     | OK     | @alenl2                |
| C7  | RX6               | ?       | ?     | OK     | @alenl2                |

---
KISS (KISSFC)
| Pin | Label             | S.Audio | Tramp | S.Port | Notes                  |
|-----|-------------------|---------|-------|--------|------------------------|
| A13 | PWM5              | ?       | ?     | NG     | @basdelfos     |
| A02 | PITCH             | ?       | ?     | OK     | @basdelfos    |
| A15 | ROLL              | ?       | ?     | OK     | @basdelfos |