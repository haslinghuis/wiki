The internal rotation convention bf uses to represent the flight controller orientation is R = Rz(-Yaw) * Ry(-Pitch) * Rx(-Roll). On one hand R describes how the flight controller with respect to the origin of the quads is rotated. On the other hand R descripes the transformation of sensor readings of the flight controller as they were measured with a flight controller aligned with the quads frame (arrow pointing forward / x axis).

For the alignment process we actually have to think in the inverse transform R^T = Rx^T(-Roll) * Ry^T(-Pitch) *  Rz^T(-Yaw). So we yaw first, then pitch the yawed frame and finally roll the yawed and pitched frame. Due to the minus sign positiv angle direction is described by the left hand rule, e.g. to yaw positive you grab the z axis with your left hand and rotate towards the direction of your fingers.

You can use the following link to figure out your board alignment angles: https://www.geogebra.org/3d/sj5aeucn

For the alignment process think of a frame mounted to your quad and a frame mounted to your flight controller. The question than then needs to be answered is how to rotate the quads frame (x pointing forward, y left, z upwards) so that the quads frame is aligned to the flight controller frame (x pointing forward on your flight controller (mind the arrow), y to the left, z upwards). Now:
1. Yaw the quad frame around its z axis
2. Pitch the already yawed frame around its y axis
3. Roll the already yawed and pitched frame around its x axis

To test your alignment process use only the fc pointing towards your monitor and the the Setup-page of the configurator and simply start with:
1. Yaw the fc, hit Reset Z axis, check the visualisation of the quad, if ok proceed
2. Pitch the yawed fc, reset Z axis, check, if ok proceed
3. Roll the yawed and pitched fc, reset Z axis, check, if ok proceed
4. Double check!