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
```
apt-get install osspd
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





