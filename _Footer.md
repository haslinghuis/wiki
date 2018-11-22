**Matek Setup for F411 mini for Tricopter** by Flashted

Setup: 2 front motors facing forward and rear motor / servo facing you
Motor 1 - rear to motor output pin S1
Motor 2 right to motor output pin S2
Motor 3 left to motor output pin S3
Motor 4 motor output pin S4 Free


On the BetaFlight GUI on the Receiver Tab is a selection for how the model is setup.
Betaflight Defaults to AETR1234 This is an accepted standard setup on Taranis
feature SERVO_TILT
feature CHANNEL_FOWARDING
Needs to be OFF.
Selecting TRICOPTER sets yaw output to servo.

Set tail servo Matek F411
resource motor 6 none  Pin on board is S6
***
B10)
resource servo 1 B10
save

Nothing needs to be changed in the Servos tab for movement.

Do not draw power from the board with a servo especially on a tricopter, so positive, black negative are going to a pdb 5v output, The yellow or white wire,signal wire can be connected to motor pin 5, motor pin 6, 7 or 8 other pins whatever will work motor pin 4 will not work for Matek F411 due to timing issue.

IF you want to disable the tail servo when it`s not armed, go to cli and type:
set tri_unarmed_servo = OFF 
or
set tri_unarmed_servo = ON
save

Now check if your servo/motor are tilting in the correct direction. If you move your yaw stick to the left, the motor must tilt to the right. If not, there are 2 ways to fix this.
In cli type:
set yaw_control_direction = -1 
save
Or you can just reverse the yaw direction on your tx, but it is better to do it in the GUI

Now check if your servo/motor are compensating in the correct direction. If you quickly move your tail to the right (So nose will tilt left), the motor must tilt quickly to the left.

An analog servo works as will a digital one in most cases my directions were not reversed with an analog servo, but may be with a digital.

