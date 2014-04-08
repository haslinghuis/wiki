# Build with DEBUG=GDB
![make debug](http://i.imgur.com/fA0m0xM.png)

# GDB and OpenOCD

start openocd:

      openocd -f /usr/share/openocd/scripts/board/stm32vldiscovery.cfg

Create a new debug configuration in eclipse :
![connect to openocd](http://i.imgur.com/somJLnq.png)
![use workspace default](http://i.imgur.com/LTtioaF.png)

you can control openocd with a telnet connection:

     telnet localhost 4444

stop the board, flash the firmware, restart:

     reset halt
     wait_halt 
     sleep 100
     poll
     flash probe 0
     flash write_image erase /home/user/git/baseflight/obj/baseflight_NAZE.hex 0x08000000
     sleep 200
     soft_reset_halt
     wait_halt
     poll
     reset halt

A this point you can launch you debug in eclispe.


# GDB and J Link
Here are some screenshots showing my configuration of Eclipse. (Kepler)

![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/debugging.PNG)

![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 1.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 2.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 3.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 4.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 5.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 6.PNG)
![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 7.PNG)

If Eclipse can't find your breakpoints and they are ignored then check your path mappings (if using cygwin) or use the other debugging provider as follows:

![](https://raw.github.com/wiki/multiwii/baseflight/images/eclipse-gdb-debugging/config 8 - If breakpoints do not work.PNG)