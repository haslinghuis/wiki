## This page will show you how to create a hex file from the source code provided by Boris. 

# Prerequisites

* A Mac that has Terminal Version 2.6.1 or later.  

* ARM GCC 4.9 series compiler 

* The `make` command implemented into Terminal. 

# The `make` Command 

 In terminal type in `make`. If you receive this, the `make` command is already implemented into terminal and you can move on to ARM GCC 4.9 install guide.

`make: *** No targets specified and no makefile found.  Stop.`,

  If you don't see it, a popup should appear asking to install the tools. Click install. If you don't see the popup type:

 `xcode-select --install`. 

 If that does not work, you will need to go to the App store on your computer and download Xcode. Go to the preferences menu, then to the downloads tab and install the "command line tool" package.


# ARM GCC 4.9 install guide

Cleanflight uses ARM GCC and it is a vital component to produce the hex file. First, go to https://launchpad.net/gcc-arm-embedded and click on "All downloads" on the right side of the page. Scroll down until you see `gcc-arm-none-eabi-4_9-2015q3-20150921-mac.tar.bz2`. Click on it so it downloads. 

Move the file to a desired spot, for example the Documents folder. Make sure you unpack the file. The directory should look something like this, if you put it in the Documents folder. 

`Users/siddha/Documents/gcc-arm-none-eabi-4_9-2015q3`

Now open terminal and type:

`nano ~/.profile`

Next type:

`export PATH=$PATH:~/Users/siddha/Documents/gcc-arm-none-eabi-4_9-2015q3`

**NOTE: You should insert the directory of your file... `~/Users/siddha/Documents/gcc-arm-none-eabi-4_9-2015q3` is an example **

Now type CTRL + X to exit out, then type y to save. 

In a new terminal window type:

`arm-none-eabi-gcc --version`

You should receive a message saying: 

`arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 4.9.3 20150529 (release) [ARM/embedded-4_9-branch revision 227977] Copyright (C) 2014 Free Software Foundation, Inc.`

You can now move on to creating the hex file. 

If you received: `arm-none-eabi-gcc: command not found` make sure you entered it in the directory correctly. If you made sure it was entered correctly and the error message still popped up, enter:

 `nano .bash_profile` instead of `nano ~/.profile`

Enter the path in again then type CTRL + X to exit out and type y to save.

Now type:

`arm-none-eabi-gcc --version`

you should see the version number pop up:

`arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 4.9.3 20150529 (release) [ARM/embedded-4_9-branch revision 227977] Copyright (C) 2014 Free Software Foundation, Inc.`

# Creating the Hex File

Go to: https://github.com/borisbstyle/betaflight/releases and download the latest Source code (tar.gz). The tar.gz file will allow you to edit the Makefile, so you can pick a target. Once the file is downloaded make sure you un pack it. 

Next, open up terminal and type:

`cd`

Now drag or type in the directory of the betaflight folder. It should look something like this:

`cd /Users/siddhakilaru1/Downloads/betaflight-2.6.0`

**NOTE: You should insert the directory of your file... `/Users/siddhakilaru1/Downloads/betaflight-2.6.0` is an example **

Now click enter, and type:

`make`

This will begin the process of making the hex file. After everything is done, go into Betaflight folder. You will see a new folder named `obj`. Inside that folder you will see you the hex file. 

# Changing the Target

If you want to change the target, go into the Betaflight folder and you will see a file called `Makefile`. 

Inside the file you will see:

`TARGET		?= NAZE `

You can change `NAZE` to any of the following options:

`NAZE NAZE32PRO OLIMEXINO STM32F3DISCOVERY CHEBUZZF3 $(CC3D_TARGETS) CJMCU EUSTM32F103RC SPRACINGF3 PORT103R SPARKY ALIENFLIGHTF1 ALIENFLIGHTF3 COLIBRI_RACE LUX_RACE MOTOLAB RMDO IRCFUSIONF3 AFROMINI SPRACINGF3MINI SPRACINGF3EVO DOGE`