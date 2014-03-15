SerialRX is not like PPMSUM which is still analog in time domain. SerialRX is a purely digital lossless protocol that uses 3 wires, signal, power, ground.

SpekSat connections are ground, input pin 4, 3.3V power (NOT 5V, use the 3.3V pin)
(todo a pic here)

For SBUS you need a hardware signal inverter. Signal goes to input pin 4, power is 5V.
(todo a pic and a link to inverter here)

SumD is the simplest, ground, input pin 4, 5V power. Set the receiver to SUMD with the transmitter.
(todo a pic here)

To enable SerialRX use CLI and type

"Feature SERIALRX"

and then choose the type of receiver by typing one of the following lines

"Set serialrx_type 0" for Spektrum 10bit (1024)

"Set serialrx_type 1" for Spektrum 11bit (2048)

"Set serialrx_type 2" for Futaba SBUS mode

"Set serialrx_type 3" for  for Graupner SUMD
