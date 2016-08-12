## How to install:

Using Betaflight Configurator, select the firmware_flasher tab and press the "Load firmware [Local]" button, you can now browse to the folder you downloaded the Betaflight firmware to & select the correct firmware hex for your corresponding Flight Controller. Leaving all options unchecked ** (defaults) press "Flash firmware".  The Configuration tool should now erase the target and flash the selected firmware to your Flight controller. Depending on your flight controller, this may prompt the installation of DFU driver, you may see a windows popup for "installing new hardware", if this happens allow it to complete and then press the "flash firmware" button again.

** Only exception to this being for the SPracingF3 flight controller, for this FC you will need to check 'Manual baud rate' and change the selection to '230400' baud.

## DFU flashing under Windows:
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

## Installing STMicro Virtual Com Port (VCP) Driver under Windows:

Many of the F4 boards (REVO, ALIENFLIGHTF4, BLUEJAYF4), and some F3 boards (SPRacingF3EVO, STM32DISCOVERY) utilise the STM32 Virtual Com Port (VCP) - a CDC serial implementation. This allows the UARTs on board to be utilised whilst the USB is connected. This requires the STM VCP driver to be installed so that the VCP to be recognised as an additional comm port on the PC. 
**NOTE:** this is similar to installing a USB serial driver, e.g. FTDI or SiLabs

The STM32 VCP driver can be downloaded here --> http://www.st.com/web/en/catalog/tools/PF257938

**NOTE:** Once you download and run the installation it has not installed the driver, merely unpacked the choice of drivers. Locate the installation directory and then run the EXE file pertaining to your system.

e.g. C:\Program Files (x86)\STMicroelectronics\Software\Virtual comport driver\Win8\ <- will have two files present. One for 64 bit systems and one for 32 bit systems.
