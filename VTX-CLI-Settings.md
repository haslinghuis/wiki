As of Betaflight version 3.3.0, the CLI settings below can be used to
configure addressable video transmitters (such as
TBS-[SmartAudio](https://github.com/betaflight/betaflight/wiki/Unify-Smartaudio)
and
IRC-[Tramp](https://github.com/betaflight/betaflight/wiki/IRC-Tramp)[)](https://github.com/betaflight/betaflight/wiki/IRC-Tramp)
that are connected to the flight controller.\
 \
 At startup the settings are applied to the transmitter.  If the video
configuration is modified via the [CMS OSD
menu](https://github.com/betaflight/betaflight/wiki/Unify-Smartaudio#smartaudio-cms-guide)
or via MSP (Taranis/OpenTX smartport
'[lua](https://github.com/betaflight/betaflight-tx-lua-scripts)'), the
settings are updated.\
 \
 One nice thing the settings can provide is a way to configure a
frequency (via USB / CLI) while the video transmitter is not powered
up.  After a save and power cycle, the system will startup at the new
frequency.\
 \
 There is a 'vtx\_freq' setting that operates as follows:  If
vtx\_band=0 and vtx\_freq!=0 then the 'vtx\_freq' value (in MHz) will be
configured on the transmitter at startup.  If both are zero then the
settings will be ignored.  If vtx\_band!=0 and a video transmitter is
connected then 'vtx\_freq' will be set to the current frequency value
(in MHz) at startup.\
 \
**vtx\_band = \#**\
 Allowed range: 0 - 5\
 0=user, 1=A, 2=B, 3=E, 4=F(Airwaves/Fatshark), 5=Raceband\
 \
**vtx\_channel = \#**\
 Allowed range: 1 - 8\
 \
**vtx\_power = \#**\
 Allowed range: 0 - 5\
 for SmartAudio:  0=25mW, 1=25mW, 2=200mW, 3=500mW, 4=800mW\
 for TBS Unify Nano:  0=25mW, 1=25mW, 2=50mW\
 for IRC-Tramp:  0=25mW, 1=25mW, 2=100mW, 3=200mW, 4=400mW, 5=600mW\
 \
**vtx\_freq = \#\#\#\#**\
 Allowed range: 0 - 5999\
 if vtx\_band!=0 and VTX connected then shows freq in MHz\
 if vtx\_band==0 then sets frequency in MHz\
 \
 if vtx\_band==0 and vtx\_freq==0 then the settings will not be sent out
to the VTX\
 \
 For example, to configure the VTX to use band 'F' and channel '6' (5840
MHz), enter the CLI and input:\
 \
 set vtx\_band = 4\
 set vtx\_channel = 6\
 save\
 \
 The VTX configuration will not be changed until after the 'save' and
restart.  If it is successful then entering 'get vtx\_freq' will show
the current frequency value in MHz.\
 \
 Frequency table:\
                                            Channel\
                  1       2       3       4       5       6      7       8\
 Band 1:  5865 5845 5825 5805 5785 5765 5745 5725  (A: Boscam A / TBS /
RC305)\
 Band 2:  5733 5752 5771 5790 5809 5828 5847 5866  (B: Boscam B)\
 Band 3:  5705 5685 5665 5645 5885 5905 5925 5945  (E: Boscam E / DJI)\
 Band 4:  5740 5760 5780 5800 5820 5840 5860 5880  (F: IRC NexWave /
Fatshark)\
 Band 5:  5658 5695 5732 5769 5806 5843 5880 5917  (R: Raceband)\
 \
 See [here for a 5.8GHz FPV "Visual" Frequency
Chart](http://www.etheli.com/freq/FPV_5.8GHz_Freqs.jpg)