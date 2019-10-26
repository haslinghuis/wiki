### OpenLog is not a suitable serial blackbox logging device for Betaflight 4.0 and beyond

As a result of never ending demands for different log items for better and more diverse flight characteristics analysis, output bandwidth requirement for serial blackbox output finally exceeded 230.4Kbps which is the fastest supported by **_OpenLog_**.

For 4.0 and beyond, serial blackbox users must use logging devices with faster communication speed such as _**OpenLager**_ instead, and configure baudrate at 1.5Mbps at least for stable recording.

See https://github.com/betaflight/betaflight/issues/9043#issuecomment-544333986 for bandwidth requirement for varying logging rates.