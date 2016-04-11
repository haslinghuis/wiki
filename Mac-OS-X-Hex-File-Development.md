***Currently, this page is under construction***

This page will show you how to create a hex file from the source code provided by Boris. 

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

# Creating the hex file


