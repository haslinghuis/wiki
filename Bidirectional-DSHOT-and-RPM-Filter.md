### Dshot & Betaflight 4.0

## Required configuration

### DMA

The current implementation requires normal DMA to be used, not burst dma. This may or may not work with a given FC. You can simply try it out:

set dshot_burst=off

And test whether your quad still flies. If so proceed to the next step:

### Enabling Bidirectional Dshot

set dshot_bidir=on

See if your motors still spin up. If so try detach the USB cable, connect a battery and reconnect USB. Now go to the CLI and type ``status``. You should see dshot telemetry being reported. The reported rpm should be zero for each motor and there should be few errors.

### Motor Poles

The escs report erpm which needs to be converted to rpm using the number of poles (magnets) of the motors. Regular 5" motors have 14 poles and that's the default setting. Smaller motors have less poles, often 12. Count them or look up the motor specs and configure using:

set motor_poles=14

### Supported FCs

OMNIBUSF4SD target (Joe)
Banggood 20mm NOXE FC (uses NOX target, n channels reassigned breaks LED support). (Joe)
Kakutef4v2 (bizmar)


### References

Bidirectional Dshot PR: https://github.com/betaflight/betaflight/pull/7271

Rpm Filter PR: https://github.com/betaflight/betaflight/pull/7264

A test version of blheli32 with erpm telemetry on signal line support is now available:
https://github.com/bitdump/BLHeli/tree/master/BLHeli_32%20ARM/BLHeli_32%20Test%20code%20Rev32.61%20hex%20files