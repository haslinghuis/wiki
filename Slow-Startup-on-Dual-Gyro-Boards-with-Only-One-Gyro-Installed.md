If you are having a slow startup of the flight controller when you plugg battery or USB, you must disable the second gyroscope by entering CLI and copying this command:
```
Set gyro_2_bustype = NONE
```
Note: this tip only works with Dualgyro based boards with only one gyroscope installed