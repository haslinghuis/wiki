# Can't change your OSD font?
## Description
Betaflight-configurator font upload fonction via USB doesn't work on some flight controllers straight forward.

![OrsarLiang](https://oscarliang.com/ctt/uploads/2017/07/betaflight-osd-font-manager.jpg)

No matter how many times you upload the font, the OSD still display the defaut font.

## Resolution
You need the battery to be **plugged in** so the fonction works properly.

## Most probable Cause
OSD chip is most likly the problem, probably sensitive to voltage.

# Concerned Board
The following board/FC are known to have font upload problem

| Board Name| Target | OSD chip | Lipo in fix issue ? |  Product URL| 
| --- | --- | --- | --- | --- |
| Dalrc F405 AIO | DALRCF405 | N/A | yes |  [dalrc](http://www.dalrc.cn/DALRC/plus/view.php?aid=186) |
| Diatone Mamba F405 Mini FC | FURYF4OSD | N/A | yes | [diatone](https://www.diatoneusa.com/store/p574/MAMBA_F405_Mini_Betaflight_Flight_Controller_F25_25A_2_4S_DSHOT600_FPV_Racing_Brushless_ESC.html) |


# Source
[Issue link](https://github.com/betaflight/betaflight-configurator/issues/1301)
