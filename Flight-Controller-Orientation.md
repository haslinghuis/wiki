The internal rotation convention bf uses to represent the flight controller orientation is R = Rz(-Yaw) * Ry(-Pitch) * Rx(-Roll). On one hand R describes how the flight controller with respect to the origin of the quads frame is rotated. On the other hand R descripes the transformation of sensor reading of the flight controller as they were measured with a flight controller aligned with the quads frame origin.

For the alignment process we actually have to think in the inverse transform R^T = Rx^T(-Roll) * Ry^T(-Pitch) *  Rz^T(-Yaw). So we yaw first, then pitch the yawed frame and finally roll the yawed and pitched frame.

* figure1 shows the quad frame and the fc frame.
* figure2 to get there we are yawing first (positiv angle is left hand rule where your thumb is pointing towards the z-axis).
* figure3 next we pitch the already yawed frame (grab y-axis with the left hand).
* figure4 then we roll the already yawed and pitched frame  (grab x-axis with the left hand), so that it aligns with the actual flight controller frame (arrow pointing towards positiv x-axis/roll).

To debug your process use the configurator and simply start with:
1. yaw, reset z axis, check
2. pitch the yawed, reset z axis, check
3. roll the yawed and pitched, reset z axis, check

https://github.com/betaflight/betaflight/wiki/Flight-Controller-Orientation