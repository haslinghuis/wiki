## Cygwin on Windows
ccache 3.1.9 in Cygwin 2.874 is broken. Returns false cache hits. Do not install. Betaflight build system uses ccache if available. 

## GNU ARM Toolchain Version
Using the 4.8-2014-q2 version of the [GNU ARM Toolchain](https://launchpad.net/gcc-arm-embedded/+download) with Windows/Cygwin (recommended on the Cleanflight "[Building in Windows.md](https://github.com/cleanflight/cleanflight/blob/master/docs/development/Building%20in%20Windows.md)" page) lead to strange results.  (When running a 2.9.1 version of Betaflight I built on NAZE the change-flight-mode beep would constantly sound when in Horizon mode.)  Updating the toolchain to 4.9-2015-q3 and rebuilding fixed the issue (and also cleared some compiler warnings).  BorisB mentions the toolchain version [here](http://www.rcgroups.com/forums/showthread.php?p=34530653#post34530653).

## Running a 32-bit Tool chain on a 64-bit Linux system
The arm-sdk installation done from the new "make arm_sdk_install" command will install a 32-bit variant of yor tool chains. This may fail to execute on a 64-bit Linux system, bash will say "No such file or directory" if no support for 32-bit 386 program are installed. To install this:

    sudo dpkg --add-architecture i386  
    sudo apt-get update  
    sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386  

