# Installing the Linux subsystem

Your PC must be running a 64-bit version of Windows 10 Anniversary Update or later (build 1607+).

You will need to restart your PC at least once, so be mentally prepared for that.

[Listening to Kraftwerk](https://www.youtube.com/watch?v=OQIYEPe6DWY) is known to increase the success rate of this process.

First, install the Linux subsystem from this great guide (from which I stole the first sentence): [https://msdn.microsoft.com/en-us/commandline/wsl/install-win10](https://msdn.microsoft.com/en-us/commandline/wsl/install-win10)

It should look like this:
![Imgur](https://i.imgur.com/uj0QPQY.jpg)

If you don't see this screen, open a command prompt and enter  `bash`

# Configuring the Linux subsystem

You will have to install a few Linux packages.

First, the basics. We need Python and Curl. To install those, enter

`sudo apt install python`

and

`sudo apt install curl`

So far so good, eh?

# Cloning betaflight

If you already have the repository cloned to your computer and do not wish to use git, you can skip this step.

To install git, enter:

`sudo apt install git`

Once that's done, navigate to the folder you want to have the repository cloned to. Use `cd [folder name]` to enter a folder, `cd ..` to go up a folder and `ls` to see the current folder's contents. Use `mkdir [foldername]` to create a new folder.

Once that's done, enter `git clone https://www.github.com/betaflight/betaflight [foldername]` This will create a new folder within the folder you are in with the name you specified and clone (=download) the Betaflight repo into it.

# Building Betaflight

To build Betaflight, you have two things left to install. Enter the folder you cloned the Betaflight repo into. The `ls` command should output something like this:
![Imgur](https://i.imgur.com/Kd65LfN.jpg)
If it doesn't match, you are in the wrong folder. Use the Windows file manager to help if you are lost.

If you are missing a few folders, don't worry, we are going to solve that right now.

First, enter `sudo apt install build-essential`. This is the package that will actually build Betaflight. Then, enter `make arm_sdk_install`.

Once that is done, your system is ready to spit out those sweet hex files.

Enter `make <targetname>` to build Betaflight for your chosen board. For example `make BETAFLIGHTF3` or even `make BETAFLIGHTF3 BETAFLIGHTF4` for multiple targets. `make help` is also your friend. 
Alternatively, you can try `make all` if you have some spare time.

The hex files will be in the `/obj` folder of the betaflight folder

## Optional items

If you do not like the default Windows console, you can always install for example `xterm`. This will however also require an X-server running in the Windows host environment. Get it here: https://sourceforge.net/projects/xming/

Then do the following in the Linux subsystem:


`$ sudo apt install xterm`   
`$ sudo apt-get install dbus-x11`   
`$ sudo dbus-uuidgen --ensure`   
`$ xterm --display :0.0 &`   
