## How to install:

Using Betaflight Configurator, select the firmware_flasher tab and press the "Load firmware [Local]" button, you can now browse to the folder you downloaded the Betaflight firmware to & select the correct firmware hex for your corresponding Flight Controller. Leaving all options unchecked ** (defaults) press "Flash firmware".  The Configuration tool should now erase the target and flash the selected firmware to your Flight controller. Depending on your flight controller, this may prompt the installation of DFU driver, you may see a windows popup for "installing new hardware", if this happens allow it to complete and then press the "flash firmware" button again.

> Only exception to this being for the SPracingF3 type of flight controllers, for those FC you *may* need to check 'Manual baud rate' and change the selection to '115200' baud.

There are basically two classes of USB devices used by all FCs:
- type 1 
1. Using a Silabs CP2103 USB interface chip.
1.1 Needs the Silabs CP210x driver. Used in both BootLoader mode for flashing and normal config mode. Shows up as a "COMx" device in BFC.
http://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers  

- type 2  
2. Using the MCU integrated STM32 VCP USB interface.
2.1 Needs WinUSB driver when in BootLoader mode, for flashing. Installed by Zadig or ImpulseRC DF. Shows up as a "DFU" device in BFC.
2.2 Needs STM VCP driver for connection and configuration with BFC Shows up as a "COMx" device in BFC.

- CC3D is a special case.  
It is an type 2 FC, but it is lacking the 2.1 USB-DFU interface as it is an STM32F1 based FC. All F1 based FC only has serial UART based bootloader interfaces. CC3D needs an external USB-serial adapter on UART1 for bootloader connection and flashing. (Or a secondary bootloader flashed, OP Bootloader for example).  

## Native USB based flight controllers - type 2
Note that this is for those controllers that are *not* using a hardware serial bridge - e.g. FTDI or SiLabs CP210x.

Driver issues can be fixed using this handy tool: https://impulserc.blob.core.windows.net/utilities/ImpulseRC_Driver_Fixer.exe

It requires .net framework v4.5. Available here: https://www.microsoft.com/en-au/download/details.aspx?id=30653

If you are having trouble connecting to your flight controller:
[![](https://img.youtube.com/vi/m4ygG6Y5zXI/0.jpg)](https://www.youtube.com/watch?v=m4ygG6Y5zXI)

### DFU flashing under Windows - USB DFU:
Make sure you have zadig if you're using Windows to enable the DFU driver. Instructions:

1. Download Zadig: http://zadig.akeo.ie/
1. Put device in DFU mode. If this is the first time to put Betaflight on you need to short the BL or BOOT pads (or press and hold the BOOT tactile button) while plugging the USB into the board.
1. Open Zadig.
1. Options > List All Devices
1. Click on the drop bown box and click the device listed STM32 BOOTLOADER
![Zadig Screenshot](https://raw.githubusercontent.com/rs2k/raceflight/raceflight/docs/assets/images/zadig-dfu.png)
1. In the box to the right of the green arrow, select WinUSB (v6.1.7600.16385)
1. Click Install Driver
1. After the install completes, restart your computer (you can cheat and ensure no browser is running - but it is not guaranteed to work). The board should stay in DFU mode - IF - usb power remains during the reboot. If not, execute step 2 again.
1. Open up the Betaflight configurator.
1. Go to firmware flasher, select "No reboot sequence"
1. On F4 targets disable "Full Chip Erase". Use the config reset in Configurator later. ([#200](https://github.com/betaflight/betaflight-configurator/issues/200) reports the issue.)
1. Load Firmware [Local]
1. Browse to and select the proper hex file. (betaflight_REVO.hex for the revo, for example)
1. Click flash firmware.
1. The board should start flashing. First indicating an erase, then flash and finally verification.
1. Once flashed your board will reboot, but you may need to install the STM VCP driver (see below) for Betaflight Configurator to connect to the board.

### Installing STMicro Virtual Com Port (VCP) Driver under Windows:

Many of the F4 boards (REVO, ALIENFLIGHTF4, BLUEJAYF4), and some F3 boards (SPRacingF3EVO, STM32DISCOVERY) utilise the STM32 Virtual Com Port (VCP) - a CDC serial implementation. This allows the UARTs on board to be utilised whilst the USB is connected. This requires the STM VCP driver to be installed so that the VCP to be recognised as an additional comm port on the PC. 
**NOTE:** this is similar to installing a USB serial driver, e.g. FTDI or SiLabs

The STM32 VCP driver can be downloaded here --> http://www.st.com/web/en/catalog/tools/PF257938

**NOTE:** Once you download and run the installation it has not installed the driver, merely unpacked the choice of drivers. Locate the installation directory and then run the EXE file pertaining to your system.

e.g. C:\Program Files (x86)\STMicroelectronics\Software\Virtual comport driver\Win8\ <- will have two files present. One for 64 bit systems (dpinst_amd64.exe) and one for 32 bit systems (dpinst_x86.exe).

### Platform Specific: Linux

Linux requires udev rules to allow write access to USB devices for users. The command bellow will create a template rule for you.

    (echo '# DFU (Internal bootloader for STM32 MCUs)'
     echo 'ACTION=="add", SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE="0664", GROUP="plugdev"') | sudo tee /etc/udev/rules.d/45-stdfu-permissions.rules > /dev/null

Now you need to find the real product id of your FC. Type in the command bellow and plug your FC in and out. It should print a line with the product id out.

    udevadm monitor --environment --udev | grep ID_MODEL_ID

Now update the entry in "/etc/udev/rules.d/45-stdfu-permissions.rules" accordingly. You can add more than one rule in the file. The default product id is the FC in bootloader mode. Then reload rules using:

    sudo udevadm control --reload-rules && udevadm trigger

You can then test the rule using when your FC is plugged in:

    udevadm test $(udevadm info -q path -n /dev/ttyACM0)

Ensure line "MODE 0664 /etc/udev/rules.d/45-stdfu-permissions.rules" is present

This assigns the device to the plugdev group(a standard group in Ubuntu). To check that your account is in the plugdev group type groups in the shell and ensure plugdev is listed. If not you can add yourself as shown (replacing <username> with your username):

    sudo usermod -a -G plugdev <username>

If you see your ttyUSB device disappear right after the board is connected, chances are that the ModemManager service (that handles network connectivity for you) thinks it is a GSM modem. If this happens, you can issue the following command to disable the service:

    sudo systemctl stop ModemManager.service

If your system lacks the systemctl command, use any equivalent command that works on your system to disable services. You can likely add your device ID to a blacklist configuration file to stop ModemManager from touching the device, if you need it for cellural networking, but that is beyond the scope of cleanflight documentation.

If you see the ttyUSB device appear and immediately disappear from the list in Cleanflight Configurator when you plug in your flight controller via USB, chances are that NetworkManager thinks your board is a GSM modem and hands it off to the ModemManager daemon as the flight controllers are not known to the blacklisted