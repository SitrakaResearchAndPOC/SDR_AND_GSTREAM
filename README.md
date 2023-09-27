# SDR MODULATION AND DEMODULATION
OS : UBUNTU 18.04

# INSTALLATION : 
```
apt-get install hackrf gnuradio rtl-sdr ffmpeg vlc gr-osmosdr
```
```
apt-get install  libtool git cmake libusb1.0-0dev build-essential libc6-dev librtlsdr-dev
```

# RTL-SDR BLACKLIST PROBLEM
```
apt update
```
```
apt-get install mousepad
```
```
cd /etc/modprobe.d/
```
```
touch backlist-rtl.conf 
```
OR (could renamed  as : /etc/modprobe.d/blacklist-rtl2832.conf ) 
```
touch blacklist-rtl2832.conf  
```
ADDING CONFIG BLACKLIST ON CONF FILE
```
mousepad blacklist-rtl2832.conf  
```
OR 
```
mousepad backlist-rtl.conf
```
ADDING THIS CONFIGURATION : 
```
    blacklist dvb_usb_rtl28xxu
    blacklist rtl2832
    blacklist rtl2830
```
CHANGING RULES
```
mousepad  /etc/udev/rules.d/rtl-sdr.rules
```
CHANGING
```
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0bda", ATTRS{idProduct}=="2832", MODE:="0666"
```
BY :
```
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0bda", ATTRS{idProduct}=="2832", MODE:="0666", GROUP="adm", SYMLINK+="rtl_sdr"
```



# SDR_AND_GSTREAM

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

# DOCUMENTATIONS RTL_SDR BLACKLIST
* https://www.reddit.com/r/homeassistant/comments/rr1guj/blacklist_rtl_sdr_drivers_modprobe_doesnt_work/?rdt=57652
* https://groups.google.com/g/ultra-cheap-sdr/c/v_GktYco0_Q
* https://www.cyberithub.com/how-to-install-librtlsdr-dev-package-on-ubuntu-20-04-lts/
* https://gist.github.com/n8acl/dab196ce30e1e2139727cb7f76d300d6
* https://medium.com/@mounoydev/rtl-sdr-rpi-4a1483016833

# DOCUMENTATIONS GSTREAM
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
