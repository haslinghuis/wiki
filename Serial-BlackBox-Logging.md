### Betaflight 4.x.x no longer supports OpenLog

As a result of never ending demands for different log items for better and more diverse flight characteristics analysis, output bandwidth requirement for serial blackbox output finally exceeded 230.4Kbps which is the fastest supported by **_OpenLog_**.

From 4.x.x and on, serial blackbox users must use _**OpenLager**_ instead, and configure baudrate at 1.5Mbps at least for stable recording.

See https://github.com/betaflight/betaflight/issues/9043#issuecomment-544333986 for bandwidth requirement for varying logging rates.
