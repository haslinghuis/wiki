# JESC: Rpm Filtering on BLHeli_S HW

Quadcopter flight performance is greatly improved by reducing motor noise in the gyro signal and by removing filter delay. Unfortunately these goals used to oppose each other: filter out more motor noise and you will get more delay.

Not any more! RPM Filtering allows Betaflight to place very narrow notch filters on the harmonics of your motors with surgical precision. Notch filters have infinite noise suppression at their center. This allows the Rpm Filter to completely remove motor noise from your FC with much less delay than we're used to.

The results are smoother and colder motors - even with bent props - and better pid loop performance due to the much reduced filter delay. The ESCs make this possible by sending the current speed of each motor at full pid loop speed to the FC where it's used to tune the filters.

Formerly this worked only with BLHeli32 ESCs - and so far only a beta is available. JESC enables BLHeli_S ESCs to fully support dshot signal line rpm telemetry at 4k pid frequency. No additional wiring required.

# Requirements

Requires an efm8bb21 based BLHeli_S ESC. You can recognize them by their name tag displayed in BLHeli Configurator: if it contains an H in the middle like F-H-30 it's supported.

# Current Status

The implementation is currently in very early access. Only try it out if you're comfortable reflashing ESC's via the C2 interface.

# Installation

* 