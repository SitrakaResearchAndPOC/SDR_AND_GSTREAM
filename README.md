# SDR_AND_GSTREAM
OS : UBUNTU 18.04

## INSTALLING GTREAM

```
apt update
```
```
sudo apt-get install gstreamer1.0-plugins-base-apps
```
```
sudo apt-get install gstreamer1.0-plugins-ugly
```
```
sudo apt-get install h24enc
```
```
sudo apt-get install gstreamer1.0-plugins-bad gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-bad-doc
```
```
sudo apt-get install ubuntu-restricted-extras
```

## INSTALLING DRIVER AUDIO 
```
sudo apt-get --purge remove linux-sound-base alsa-base alsa-utils
```
```
sudo apt-get install linux-sound-base alsa-base alsa-utils
```
```
sudo apt-get install dkms build-essential linux-headers-`uname -r` alsa-base alsa-firmware-loaders alsa-oss alsa-source alsa-tools  alsa-tools-gui alsa-utils alsamixergui
```
```
sudo alsa force-reload
```
Please, run in the terminal:
```
alsamixer
```
You will then be presented with a screen like this:
![image](https://github.com/SitrakaResearchAndPOC/SDR_AND_GSTREAM/blob/main/ZYihg.png)
lease, check if the speakers or any audio output is muted and unmute it.

<i>MM</i> means mute and <i>OO</i> means unmute.

Then exit when done by pressing Esc
```
apt-get install osspd
```
## FINDING CAMERA DEVICES 
ls /dev/video*  
## FINDING AUDIO DEVICES
finding card sound options [link](https://superuser.com/questions/53957/what-do-alsa-devices-like-hw0-0-mean-how-do-i-figure-out-which-to-use):   
The hw:X,Y comes from this mapping of your hardware -- in this case, X is the card number, while Y is the device number.  
```
aplay -l
```
or [link](https://help.ubuntu.com/community/UbuntuStudio/UsbAudioDevices)
```
cat /proc/asound/cards 
```

## TESTING CAPTURING VIDEO WITHOUT SOUND
ctrl+shift + T : 
```
gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts
```
ctrl+shift + T : 
```
gst-play-1.0 video1.ts 
```

## TESTING CAPTURING VIDEO WITHOUT SOUND (MORE OPTIONS)
ctrl+shift + T : 
```
gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=400 bottom=0 !  queue ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux  ! filesink location=video1.ts
```
ctrl+shift + T : 
```
gst-play-1.0 video1.ts 
```

## TESTING CAPTURING VIDEO WITHOUT SOUND (MORE OPTIONS 2)
ctrl+shift + T : 
```
gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise !  x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts
```
ctrl+shift + T : 
```
gst-play-1.0 video1.ts 
```
## TESTING AUDIO ONLY
ctrl+shift + T : 
```
gst-launch-1.0 alsasrc device="hw:0,0" ! audio/x-raw,format=S16LE  ! wavenc  !  filesink location = audio1.wav
```
ctrl+shift + T : 
```
gst-play-1.0 audio1.wav 
```

## TESTING AUDIO ONLY (MORE OPTIONS STOP AFTER 1000 buffers)
ctrl+shift + T : 
```
gst-launch-1.0 alsasrc num-buffers=1000 device="hw:0,0" ! audio/x-raw,format=S16LE  ! wavenc  !  filesink location = audio1.wav
```
ctrl+shift + T : 
```
gst-play-1.0 audio1.wav 
```


## TESTING VIDEO MP4 (VIDEO + AUDIO) [link](https://stackoverflow.com/questions/49270207/how-to-record-audio-and-video-in-gstreamer)
ctrl+shift + T : 
```
gst-launch-1.0 -e v4l2src device="/dev/video0" ! videoconvert ! queue ! x264enc tune=zerolatency ! mux. alsasrc device="hw:0" ! queue ! audioconvert ! audioresample ! voaacenc ! aacparse ! qtmux name=mux ! filesink location=video1.mp4 sync=false
```
ctrl+shift + T : 
```
gst-play-1.0 video1.mp4 
```

## TESTING VIDEO .TS WITH AUDIO
```
gst-launch-1.0 -e v4l2src device="/dev/video0" ! videoconvert ! queue ! x264enc tune=zerolatency bitrate=498 ! mux. alsasrc device="hw:0" ! queue ! audioconvert ! audioresample ! voaacenc ! aacparse ! mpegtsmux  name=mux ! filesink location=video1.ts
```
ctrl+shift + T : 
```
gst-play-1.0 video1.ts 
```

## FFMPEG FOR CONVERTING WAV FILE [link](https://askubuntu.com/questions/919788/convert-mp3-file-to-wav-using-the-command-line)
```
ffmpeg -i input.mp3 output.wav
```
or
```
mpg123 -w output.wav input.mp3
```

## FFMPEG FOR CONVERING TS FILE [link](https://stackoverflow.com/questions/58958193/ffmpeg-video-not-converting-from-mp4-to-ts-format)
```
ffmpeg -i video1.mp4 -c:v libx264 -c:a aac -b:a 160k -bsf:v h264_mp4toannexb -f mpegts -crf 32 pqr.ts
```
This second command, sometimes doesn't run :  
```
ffmpeg -i input.mp4 -c copy -bsf h264_mp4toannexb output.ts
```

# DOCUMENTATIONS 
* https://xilinx.gitlab.avnet.com/petalinux/gstreamer-reference/gstreamer/ubuntu/ubuntu_18.04_bionic_install/
* https://stackoverflow.com/questions/69269127/gstreamer-combine-video-and-sound-from-different-sources-and-broadcast-to-rtmp
* https://docs.xilinx.com/r/en-US/ug1449-multimedia/Streaming-Out-Using-RTP-Unicast-Example
* https://askubuntu.com/questions/1032431/ubuntu-18-04-no-sound-card-detected
* https://askubuntu.com/questions/1165625/ubuntu-18-04-audio-doesnt-work-unless-i-switch-between-outputs
* https://stackoverflow.com/questions/1040233/how-to-know-what-is-the-default-audio-device-dev-audio-or-dev-dsp-in-ubuntu
* https://superuser.com/questions/53957/what-do-alsa-devices-like-hw0-0-mean-how-do-i-figure-out-which-to-use
* https://help.ubuntu.com/community/UbuntuStudio/UsbAudioDevices
* https://stackoverflow.com/questions/1040233/how-to-know-what-is-the-default-audio-device-dev-audio-or-dev-dsp-in-ubuntu
* https://stackoverflow.com/questions/49270207/how-to-record-audio-and-video-in-gstreamer
* https://support.xilinx.com/s/question/0D52E00006hplh1SAA/zcu104-warning-erroneous-pipeline-could-not-link-v4l2src0-to-sdxopticalflow0?language=en_US
* https://gstreamer.freedesktop.org/documentation/videomixer/index.html?gi-language=c
* http://www.wu.ece.ufl.edu/projects/wirelessVideo/project/H264_USRP/index.htm
* https://groups.google.com/g/baudline/c/mPUSoD7zTEY
* https://byjus.com/jee/amplitude-modulation/
* https://help.ubuntu.com/community/UbuntuStudio/UsbAudioDevices
* https://stackoverflow.com/questions/58958193/ffmpeg-video-not-converting-from-mp4-to-ts-format
