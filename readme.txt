rm -rf /var/lib/dpkg/lock-frontend 
rm -rf /var/lib/dpkg/
mkdir /var/lib/dpkg
touch /var/lib/dpkg/status
mkdir  /var/lib/dpkg/updates 
mkdir /var/lib/dpkg/info
touch /var/lib/dpkg/info/format-new
mkdir /var/lib/dpkg/alternatives

touch /var/lib/dpkg/available

COMMANDE FANAMPINY : 
ffmpeg -i video1.mp4 -c:v libx264 -c:a aac -b:a 160k -bsf:v h264_mp4toannexb -f mpegts -crf 32 pqr.ts

lien [https://stackoverflow.com/questions/58958193/ffmpeg-video-not-converting-from-mp4-to-ts-format]
 
apt update
# Active python 3 : 

sudo apt-get install gstreamer1.0-plugins-base-apps
sudo apt-get install gstreamer1.0-plugins-ugly
sudo apt-get install h24enc
sudo apt-get install gstreamer1.0-plugins-bad gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-bad-doc
sudo apt-get install ubuntu-restricted-extras
history


AUDIO DRIVER : 

sudo apt-get --purge remove linux-sound-base alsa-base alsa-utils
sudo apt-get install linux-sound-base alsa-base alsa-utils
sudo apt-get install dkms build-essential linux-headers-`uname -r` alsa-base alsa-firmware-loaders alsa-oss alsa-source alsa-tools  alsa-tools-gui alsa-utils alsamixergui

apt-get install osspd


ls /dev/video*


Video only
ctrl+shift + T : 
gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts


Video only with more option : 
gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=400 bottom=0 !  queue ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux  ! filesink location=video1.ts


Video only with more option 2 : 
gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise !  x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts


Audio only hw:0,0
gst-launch-1.0 alsasrc num-buffers=1000 device="hw:0,0" ! audio/x-raw,format=S16LE  ! wavenc  !  filesink location = a.wav



Mandeha mp4 avec video et audio : https://stackoverflow.com/questions/49270207/how-to-record-audio-and-video-in-gstreamer
gst-launch-1.0 -e v4l2src device="/dev/video0" ! videoconvert ! queue ! x264enc tune=zerolatency ! mux. alsasrc device="hw:0" ! queue ! audioconvert ! audioresample ! voaacenc ! aacparse ! qtmux name=mux ! filesink location=test.mp4 sync=false


Mandeha video avec video et audio
gst-launch-1.0 -e v4l2src device="/dev/video0" ! videoconvert ! queue ! x264enc tune=zerolatency bitrate=498 ! mux. alsasrc device="hw:0" ! queue ! audioconvert ! audioresample ! voaacenc ! aacparse ! mpegtsmux  name=mux ! filesink location=video1.ts


gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts ! 

Sary fotsiny mandeha : 
gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise !  queue ! x264enc  tune=zerolatency bitrate=498 ! mpegtsmux  ! filesink location=video1.ts



gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise !  x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! alsasrc device="hw:0,0" ! queue ! audioconvert ! audioresample ! audio/x-raw,rate=44100 ! queue ! voaacenc bitrate=128000 ! audio/mpeg ! aacparse ! audio/mpeg, mpegversion=4 ! filesink location=video1.ts



gst-launch-1.0 v4l2src device=/dev/video0 io-mode=4 ! \
video/x-raw, format=NV12, width=3840, height=2160, framerate=60/1 ! \
omxh265enc qp-mode=auto gop-mode=basic gop-length=60 b-frames=0 \
target-bitrate=60000 num-slices=8 control-rate=constant prefetch-buffer=true \
low-bandwidth=false filler-data=true cpb-size=1000 initial-delay=500 ! \
video/x-h265, profile=main, alignment=au ! queue ! mux. alsasrc device=hw:2,1 ! \
audio/x-raw, format=S24_32LE, rate=48000, channels=2 ! queue ! \
audioconvert ! audioresample ! faac ! aacparse ! mpegtsmux name=mux ! \
filesink location="/run/media/sda/test.ts"

!  flvmux name=mux streamable=true !

alsasrc device="hw:1,0" ! queue ! audioconvert ! audioresample ! audio/x-raw,rate=44100 ! queue ! voaacenc bitrate=128000 ! audio/mpeg ! aacparse ! audio/mpeg, mpegversion=4 


gst-launch-1.0 -e v4l2src device=/dev/v4l/by-path/platform-fe801000.csi-video-index0 ! video/x-raw,format=UYVY,framerate=20/1 ! videoconvert ! videoscale ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise !  x264enc  tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts

gst-launch-1.0 -e v4l2src device=/dev/video0 ! videoconvert ! video/x-raw, width=1280,height=720 ! videocrop top=0 left=0 right=800 bottom=0 ! videoflip method=counterclockwise ! omxh264enc ! h264parse!  flvmux name=mux streamable=true ! rtmpsink sync=true async=true location='rtmp://XXXXXXXXXXXXXXXX' alsasrc device="hw:1,0" ! queue ! audioconvert ! audioresample ! audio/x-raw,rate=44100 ! queue ! voaacenc bitrate=128000 ! audio/mpeg ! aacparse ! audio/mpeg, mpegversion=4 ! mux.

ctrl+shift + T : 
gst-play-1.0 video1.ts 


https://stackoverflow.com/questions/69269127/gstreamer-combine-video-and-sound-from-different-sources-and-broadcast-to-rtmp
https://docs.xilinx.com/r/en-US/ug1449-multimedia/Streaming-Out-Using-RTP-Unicast-Example


https://askubuntu.com/questions/1032431/ubuntu-18-04-no-sound-card-detected
https://askubuntu.com/questions/1165625/ubuntu-18-04-audio-doesnt-work-unless-i-switch-between-outputs
https://stackoverflow.com/questions/1040233/how-to-know-what-is-the-default-audio-device-dev-audio-or-dev-dsp-in-ubuntu
Note that ALSA devices need not directly correspond to hardware devices (like OSSv3 did), they are much more flexible and can include additional filter chains, route to multiple hardware devices, etc. Usually, the default "default" chain is linked to the first sound card device, cf. also /proc/asound/cards, and the device node is usually something like /dev/snd/pcmC0D0p

https://groups.google.com/g/baudline/c/mPUSoD7zTEY



finding card sound options : https://superuser.com/questions/53957/what-do-alsa-devices-like-hw0-0-mean-how-do-i-figure-out-which-to-use 
The hw:X,Y comes from this mapping of your hardware -- in this case, X is the card number, while Y is the device number.
aplay -l


Not very good : https://help.ubuntu.com/community/UbuntuStudio/UsbAudioDevices
cat /proc/asound/cards 

LINK : 
https://stackoverflow.com/questions/1040233/how-to-know-what-is-the-default-audio-device-dev-audio-or-dev-dsp-in-ubuntu
https://stackoverflow.com/questions/49270207/how-to-record-audio-and-video-in-gstreamer
https://support.xilinx.com/s/question/0D52E00006hplh1SAA/zcu104-warning-erroneous-pipeline-could-not-link-v4l2src0-to-sdxopticalflow0?language=en_US
https://gstreamer.freedesktop.org/documentation/videomixer/index.html?gi-language=c
http://www.wu.ece.ufl.edu/projects/wirelessVideo/project/H264_USRP/index.htm
https://groups.google.com/g/baudline/c/mPUSoD7zTEY
https://byjus.com/jee/amplitude-modulation/
Official site: https://xilinx.gitlab.avnet.com/petalinux/gstreamer-reference/gstreamer/ubuntu/ubuntu_18.04_bionic_install/
https://help.ubuntu.com/community/UbuntuStudio/UsbAudioDevices


-------------------------------------------------------------------------------------------
FFT tutorial in gnuradio - YouTube
https://www.youtube.com/watch?v=E7RjvDOudIo
https://www.youtube.com/watch?v=WKJBo0SzP3Q



link : 
https://ukdiss.com/examples/implementation-of-modulation-schemes.php
https://www.google.com/search?q=ofdm+%2B+audio+source&tbm=isch&ved=2ahUKEwix2t6HpI6BAxU7vycCHX9uD3QQ2-cCegQIABAA&oq=ofdm+%2B+audio+source&gs_lcp=CgNpbWcQAzoECAAQHjoGCAAQCBAeUK4CWJseYIkfaABwAHgAgAHtAogB2yiSAQYyLTEyLjaYAQCgAQGqAQtnd3Mtd2l6LWltZ8ABAQ&sclient=img&ei=f2b0ZPGeAbv-nsEP_9y9oAc&client=ubuntu&hs=UkS
https://wiki.gnuradio.org/index.php/Basic_OFDM_Tutorial
https://classes.engineering.wustl.edu/~jain/cse574-22/ftp/ofdm_sdr/index.html
https://ez.analog.com/adieducation/university-program/f/q-a/116538/problems-with-modem-ofdm-with-gnuradio-and-plutosdr
https://wireless-square.com/2016/01/14/fm-receiver-using-gnu-radio/
https://www.youtube.com/watch?app=desktop&v=kplMaL7e2l4
https://arxiv.org/pdf/2302.05775.pdf
https://www.researchgate.net/figure/Implementation-of-OFDM-modulation-and-demodulation-with-Channel-model_fig2_275720911
file:///home/oainr/Downloads/An_Experimental_Study_on_Channel_Estimation_and_Sy.pdf
https://www.youtube.com/watch?app=desktop&v=LZDWfnrxo6c
https://www.semanticscholar.org/paper/Implementation-of-OFDM-systems-using-GNU-Radio-and-Nguyen/733847b3bc1b129791dcf7edce0215dcdec73766
https://honors.libraries.psu.edu/files/final_submissions/4410  (interessant)
https://www.youtube.com/watch?v=tj_9p_rXULM
https://github.com/gallicchio/learnSDR
https://gnuradio.blogspot.com/2013/12/discuss-gnuradio-qam-modulation.html
https://dsp.stackexchange.com/questions/68306/16-qam-gnu-radio
https://dsp.stackexchange.com/questions/71310/packet-encoder-gnu-qpsk-transmission
file:///home/oainr/Downloads/125984561.pdf



https://github.com/fperal/gnuradio?search=1
https://github.com/rtlsdrblog/rtl-sdr-blog
https://github.com/timkim0713/RFJamming-FMRadio-SDR

--------------------------------------------------------------------------------------------
OLD HISTORY : 
  319  sudo apt-get install gstreamer1.0
  320  apt update
  321  sudo apt-get install gstreamer1.0
  322  rm -rf /var/lib/dpkg/lock-frontend 
  323  apt update
  324  sudo apt-get install gstreamer1.0
  325  rm -rf /var/lib/dpkg/
  326  sudo apt-get install gstreamer1.0
  327  rm -rf /var/lib/dpkg/
  328  apt update
  329  mkdir /var/lib/dpkg
  330  apt update
  331  touch /var/lib/dpkg/status
  332  apt update
  333  sudo apt-get install gstreamer1.0
  334  sudo apt-get install gstreamer
  335  sudo apt-get install gstreamer*
  336  gstream
  337  sudo apt-get install gstreamer1.0
  338  gst-launch
  339  sudo apt-get install gstreamer1.0-plugins-base-apps
  340  touch /var/lib/dpkg/updates
  341  sudo apt-get install gstreamer1.0-plugins-base-apps
  342  rm -rf /var/lib/dpkg/updates 
  343  mkdir -rf /var/lib/dpkg/updates 
  344  mkdir -f /var/lib/dpkg/updates 
  345  mkdir  /var/lib/dpkg/updates 
  346  sudo apt-get install gstreamer1.0-plugins-base-apps
  347  mkdir /var/lib/dpkg/info
  348  touch /var/lib/dpkg/info/format-new
  349  touch /var/lib/dpkg/alternatives
  350  sudo apt-get install gstreamer1.0-plugins-base-apps
  351  rm -rf /var/lib/dpkg/alternatives
  352  mkdir /var/lib/dpkg/alternatives
  353  sudo apt-get install gstreamer1.0-plugins-base-apps
  354  gst-launch-1.0 
  355  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  356  gst-launch-1.0 -v v4l2src device="/dev/video1" | videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  357  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  358  mpegtsmux
  359  sudo apt-get install gstreamer1.0-plugins-ugly
  360  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  361  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | h264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  362  apt-get install h24enc
  363  apt-get install h264enc
  364  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | h264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  365  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  366  gst-launch-1.0 -v v4l2src device="/dev/video0" | videoconvert | h264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  367  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert | x264enc tune=zerolatency bitrate=498 | mpegtsmux | filesink location=video1.ts
  368  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! h264enc tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts
  369  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts
  370  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc tune=zerolatency bitrate=498 !  filesink location=video1.ts
  371  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! h264enc tune=zerolatency bitrate=498 !  filesink location=video1.ts
  372  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc tune=zerolatency bitrate=498 !  filesink location=video1.ts
  373  gst-launch-1.0 -v v4l2src device="/dev/video0" ! videoconvert ! x264enc tune=zerolatency bitrate=498 ! mpegtsmux ! filesink location=video1.ts
  374  apt-get install  gst-plugins-bad
  375  sudo apt-get install gstreamer1.0-plugins-bad gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-bad-doc
  376  apt-get install  gst-plugins-bad
  377  sudo apt-get install gstreamer1.0-plugins-bad gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-bad-doc
  378  sudo apt install ubuntu-restricted-extras
  379  mkdir /var/lib/dpkg/available
  380  sudo apt install ubuntu-restricted-extras
  381  rm -rf /var/lib/dpkg/available
  382  touch /var/lib/dpkg/available
  383  sudo apt install ubuntu-restricted-extras
  384  history




