## HoTT telemetry

Enable HoTT telemetry on softserial port 1 with the following commands:

```
feature PPM
feature SOFTSERIAL
feature TELEMETRY
set softserial_baudrate=19200
set telemetry_port=1
set telemetry_provider=1
```

Connect Graupner HoTT receiver as follows

```
HoTT telemetry TX/RX -> Serial RX (connect directly)
Serial TX -> 1N4148 Diode -(|  )-> HoTT TX/RX (connect via diode)
Receiver Ground -> Flight Controller Ground

The diode should be arranged to allow the data signals to flow the right way
-(|  )- == Diode, | indicates cathode marker.
```

As noticed by Skrebber the GR-12 (and probably GR-16/24, too) are based on a PIC 24FJ64GA-002, which digitals pins are 5V tolerant.

Note: The softserial ports are not listed as 5V tolerant in the STM32F103xx data sheet pinouts and pin description section.  Verify if you require a 5v/3.3v level shifters.  The softserial port should not be inverted.

## FrSky on softserial

```
feature PPM
feature SOFTSERIAL
feature TELEMETRY
set softserial_baudrate=9600
set softserial_1_inverted=1
set telemetry_port=1
set telemetry_provider=0
```

Connect FrSky receiver as follows

```
Telemetry port RXD -> Serial TX (connect directly)
Receiver Ground -> Flight Controller Ground
```

Video demonstration of FrSky telemetry via Softserial

https://www.youtube.com/watch?v=dahGlc3Tf40

Turnigy 9XR wiring details

https://plus.google.com/+DominicClifton/posts/bBLiy4czD8y
