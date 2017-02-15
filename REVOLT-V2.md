REVOLT V2

Is not currently supported by BetaFlight. 

Blckman has noted it WILL be added to 3.1.6 build. However, if you want to test now, you must do the following to get it to function.

1) Download this hex until 3.1.6 is released.
https://www.dropbox.com/s/xu999lo5lhbc23a/betaflight_3.2.0_REVOLT.hex?dl=0

2) Solder the pads on the bottom of the board to enable UART 1 for Inversion See this photo.

3) Solder up the cable for the receiver on UART 1 as shown in the revolt V2 diagram (make sure its the V2 diagram)
https://revoltfc.com/pinout-diagram.html

4) Do this command in CLI.
set serialrx_halfduplex=ON

5) Hit Save.

Enjoy 

