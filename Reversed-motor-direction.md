# Running with reversed motors in betaflight (Spinning outwards at the front and back)

This page is primarily for myself since I always forget which command it is.

The motors should be spinning outwards, that is the front left spins counterclockwise, front right clockwise:


      ^    ^
     4|    |2
       \   /
         x
    ^  /   \ ^  
    |3      1|

To enable this, the command given is (as of betaflight 3.2):

    set YAW_MOTORS_REVERSED=ON

For older betaflight:

    set YAW_MOTOR_DIRECTION=-1

