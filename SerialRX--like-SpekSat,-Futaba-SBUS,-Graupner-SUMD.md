SerialRX is not like PPMSUM which is still analog in time domain. SerialRX is a purely digital lossless protocol that uses 3 wires, signal, power, ground.

SumD is the simplest, ground, input pin 4, 5V power. Set the receiver to SUMD with the transmitter.
![](http://imgur.com/W6Qnz8b.jpg)
![](http://imgur.com/a2D7z0I.jpg)

SpekSat connections are ground, input pin 4, 3.3V power (NOT 5V, use the 3.3V pin)
See here where the 3.3V pin is [Rev5 pdf](http://www.abusemark.com/downloads/naze32_rev3.pdf)
Or you use a 3.3V adapter cable [3.3V adapter cable](http://www.hobbyking.com/hobbyking/store/__24524__ZYX_S_DSM2_DSMJ_Satellite_Receiver_Cable.html)

For SBUS you need a hardware signal inverter. Signal goes to input pin 4, power is 5V.
[Cheap SBUS inverter](http://www.hobbyking.com/hobbyking/store/__24523__ZYX_S_S_BUS_Connection_Cable.html)

To enable SerialRX use CLI and type<br>
"Feature SERIALRX"<br>
and then choose the type of receiver by typing one of the following lines<br>
"Set serialrx_type 0" for Spektrum 10bit (1024)<br>
"Set serialrx_type 1" for Spektrum 11bit (2048)<br>
"Set serialrx_type 2" for Futaba SBUS mode<br>
"Set serialrx_type 3" for  for Graupner SUMD<br>
