### Black Box Logger
RCGroups Thread with links to OpenLogger and firmware:   
https://www.rcgroups.com/forums/showthread.php?2299805-Blackbox-flight-data-recorder-feature-for-Baseflight-Cleanflight 

###BlackBox Viewer
The Latest Viewer and source is here:   
https://github.com/betaflight/blackbox-log-viewer  

###Videos and tutorials
Joshua Bardwell just started a new video series which he calls BlackBox 101:   
https://www.youtube.com/playlist?list=PLwoDb7WF6c8l0-ABsIcnJt1FyhX9MBoVW   

###RCG threads on learning to analyze BB logs:  
[Joshua's Blackbox Log Video Responses](https://www.rcgroups.com/forums/showthread.php?2484202-Blackbox-Log-Video-Responses)  
[Blackbox log analyzation\help thread](https://www.rcgroups.com/forums/showthread.php?2386267-Blackbox-log-analyzation-help-thread)  

###Notes on new Black Box Viewer
If you open a Betaflight Log, then the logo shown is the Betaflight Logo and the colour scheme is orange; if you open an iNav log, then the logo changes to iNav and the colour scheme is blue; if you open a Cleanflight log (or if it can't tell what kind of firmware you were running e.g. an old version of Betaflight perhaps) then the logo shows the Cleanflight logo and the colour scheme is green....
Some features are automatically disabled depending upon the firmware you are running that creates the log. 

Do click on all the buttons to learn what they do and '?' for the Keyboard Short cuts. 

### How to obtain data to evaluate noise frequency for setting the Notch filter.

1. Use this CLI setting: "set debug_mode = notch"  
Make sure your blackbox logging rate is at least 1khz. The logging rate is based on pid-loop so 1/4 for 4k pid loop would be enough.
2. Fly as usual
3. Open your log in blackbox viewer
4. Add all debug options to your graph setup
5. Click on debug[0]  Note: This action of clicking on the graph section traces to the right will show the analyzer screen :D 
6. Make the graph fullscreen (next to playback options)
7. You can now see where your motor noise is most significant

Two images showing how to view the spectrum and the result of the notch filter.
![How to view Spectrum](https://cloud.githubusercontent.com/assets/17462561/17593758/43dbdefa-5fe7-11e6-9fa5-bd8e5f54e710.jpg)
![Filter result](https://cloud.githubusercontent.com/assets/17462561/17593764/45ec1a84-5fe7-11e6-80fd-861efeb56827.jpg)

The debug setting will log additional data to debug[0]-debug[3]:
* debug[0] is unfiltered and raw gyro data on roll axis.
* debug[1] is only notch filtered gyro data on roll axis.
* debug[2] is unfiltered and raw gyro data on pitch axis.
* debug[3] is only notch filtered gyro data on pitch axis.

Super simple visual explanation of the gyro data sequence through the filters:  
raw gyro->(debug gyro here)->soft lpf->(debug notch here)->notch1->notch2  

If the notch filter is disabled 0/1 and 2/3 will be identical. Otherwise you can directly see what the filter does.
More details on phase shift for example can be found here: https://github.com/betaflight/betaflight/pull/668

Post by r.a.v.
You can view earlier BB logs (pre BF 3.0) but the analyzer won't know at which rate it was recorded. This will probably result in a correct looking spectrum but with incorrect frequency labels. Clicking on the traces on the Graph section to the right will show the analyzer screen.

Post by ctzsnooze  
All filters add delay. Doubling slope on an IIR LPF doubles delay since the same 1st order filter is simply applied twice. None currently are FIR. FIR were evaluated and not as good as simple IIR. Dterm is IIR, gyro cut was biquad (i think it still is). There is a recent post about the notch filter that linked to the GitHub page where the Notch was discussed before implementation. Diagrams there show delay for different filter combinations. Lots of thought has gone into current filter design. 

Another post by ctzsnooze on using the Filters available in 3.x
http://www.rcgroups.com/forums/showpost.php?p=35764414&postcount=38600

