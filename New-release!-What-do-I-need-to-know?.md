This page is meant to provide a more in depth explanation of the changelog for each release so that users can understand the consequences of the changes made. Please remember to read the changelog for each release first.

# 2.3.4

Softer D approach as default baseline. Goal is to eliminate more D noise which should allow for higher D values for noisier quads. Rc_smoothing disabled by default as the new gyro delta approach should smooth out D anyway. See 2.3.3 for important details.

# 2.3.3

A lot of improvements and changes in this release. 3D airmode is much improved. Use set gyro_lpf = off to allow for 2khz mode. On F1 targets remember to also disable mag/baro/acc. **IMPORTANT: Lower your D values (to 0-5) when first enabling 2khz mode and check motor temps frequently! Some users have reported cooked motors as a result of D values that are too high.**

#2.1.6

A stable release. Use this if you are just getting started with betaflight and do not want 2khz mode or 3D airmode.