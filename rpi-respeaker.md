
```
import pyaudio

p = pyaudio.PyAudio()
info = p.get_host_api_info_by_index(0)
numdevices = info.get('deviceCount')

for i in range(0, numdevices):
    if (p.get_device_info_by_host_api_device_index(0, i).get('maxInputChannels')) > 0:
        print "Input Device id ", i, " - ", p.get_device_info_by_host_api_device_index(0, i).get('name')
```


```
python test_respeaker.py
ALSA lib confmisc.c:1281:(snd_func_refer) Unable to find definition 'cards.bcm2835.pcm.front.0:CARD=0'
ALSA lib conf.c:4528:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5007:(snd_config_expand) Evaluate error: No such file or directory

...

Cannot connect to server socket err = No such file or directory
Cannot connect to server request channel
jack server is not running or cannot be started
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
Input Device id  2  -  ReSpeaker MicArray UAC2.0: USB Audio (hw:1,0)
```

正在遍历设备

Those are PortAudio's attempts to enumerate devices. Ignore them. – CL. Mar 5 '15 at 9:49


```
import pyaudio
import wave

RESPEAKER_RATE = 16000
RESPEAKER_CHANNELS = 2
RESPEAKER_WIDTH = 2
# run getDeviceInfo.py to get index
RESPEAKER_INDEX = 1
CHUNK = 1024
RECORD_SECONDS = 5
WAVE_OUTPUT_FILENAME = "output.wav"

p = pyaudio.PyAudio()

stream = p.open(
            rate=RESPEAKER_RATE,
            format=p.get_format_from_width(RESPEAKER_WIDTH),
            channels=RESPEAKER_CHANNELS,
            input=True,
            input_device_index=RESPEAKER_INDEX,)

print("* recording")

frames = []

for i in range(0, int(RESPEAKER_RATE / CHUNK * RECORD_SECONDS)):
    data = stream.read(CHUNK)
    frames.append(data)

print("* done recording")

stream.stop_stream()
stream.close()
p.terminate()

wf = wave.open(WAVE_OUTPUT_FILENAME, 'wb')
wf.setnchannels(RESPEAKER_CHANNELS)
wf.setsampwidth(p.get_sample_size(p.get_format_from_width(RESPEAKER_WIDTH)))
wf.setframerate(RESPEAKER_RATE)
wf.writeframes(b''.join(frames))
wf.close()
```


```
jack server is not running or cannot be started
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
Expression 'parameters->channelCount <= maxChans' failed in 'src/hostapi/alsa/pa_linux_alsa.c', line: 1514
Expression 'ValidateParameters( inputParameters, hostApi, StreamDirection_In )' failed in 'src/hostapi/alsa/pa_linux_alsa.c', line: 2818
Traceback (most recent call last):
  File "test_rp2.py", line 16, in <module>
    input=True, input_device_index=RESPEAKER_INDEX, )
  File "/usr/lib/python2.7/dist-packages/pyaudio.py", line 750, in open
    stream = Stream(self, *args, **kwargs)
  File "/usr/lib/python2.7/dist-packages/pyaudio.py", line 441, in __init__
    self._stream = pa.open(**arguments)
IOError: [Errno -9998] Invalid number of channels
```

RESPEAKER_INDEX 修改为 2

生成``output.wav``


```
sudo apt install mplayer
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libdirectfb-1.2-9 libenca0 libvorbisidec1 libxvmc1
Suggested packages:
  mplayer-doc netselect | fping
The following NEW packages will be installed:
  libdirectfb-1.2-9 libenca0 libvorbisidec1 libxvmc1 mplayer
0 upgraded, 5 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,498 kB of archives.
After this operation, 5,330 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

```
mplayer output.wav
```

```
sudo apt-get install python-pyaudio python3-pyaudio sox
sudo pip install pyaudio

wget https://s3-us-west-2.amazonaws.com/snowboy/snowboy-releases/rpi-arm-raspbian-8.0-1.1.1.tar.bz2
tar -xvf rpi-arm-raspbian-8.0-1.1.1.tar.bz2
cd rpi-arm-raspbian-8.0-1.1.1/
```

```
python demo.py
```

报错：


```
ImportError: libf77blas.so.3: cannot open shared object file: No such file or directory
```

```
sudo apt-get install libatlas-base-dev
```

测试

```
python demo.py resources/snowboy.umdl
```

```
Cannot connect to server request channel
jack server is not running or cannot be started
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
Listening... Press Ctrl+C to exit
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:07
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:32
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:35
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:37
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:46
INFO:snowboy:Keyword 1 detected at time: 2017-08-26 15:10:59
```

下载模型：

```
https://snowboy.kitt.ai/hotword/351
```


--------------------------------------

 - Kaldi
 - HTK
 - [Lucida](https://github.com/claritylab/lucida)

[https://my.oschina.net/jamesju/blog/116151](https://my.oschina.net/jamesju/blog/116151)

A：创建语料库，brightness, channel,color各录制5次。
B：声学分析，把wavform的声音文件转换为mfcc格式。
C：模型定义，为词典里面的每一个词建立一个HMM原型。
D：模型训练，HMM模型初始化和迭代。
E：问题定义，即语法定义。
F：对测试结合进行识别
G：评测  

当然，如果主要是用神经网络来做识别的话，还有TensorFlow、CNTK、Theano、Torch等都是可以的，我也曾经使用过 CMU Yajie Miao 开发的 PDNN 和 KaldiPDNN（网址：yajiemiao (Yajie Miao)）。

```
pi@raspberrypi:~/kaldi-master/tools $ make
```

Software packages required
---

The following is a non-exhaustive list of some of the packages you need in order to install Kaldi. The full list is not important since the installation scripts will tell you what you are missing.

Git: this is needed to download Kaldi and other software that it depends on.
wget is required for the installation of some non-Kaldi components described below
The example scripts require standard UNIX utilities such as bash, perl, awk, grep, and make.
It can also be helpful if you have an ATLAS linear-algebra package installed on your system. Most systems already have this (You can also search the packages in linux for installation by simple commands like "yum search atlas" or "apt-cache search libatlas"); the best approach is to ignore this requirement for now and see if you have problems when you install Kaldi.
```


Software packages installed by Kaldi
---

OpenFst: we compile against this and use it heavily.
IRSTLM: this a language modeling toolkit. Some of the example scripts require it but it is not tightly integrated with Kaldi; we can convert any Arpa format language model to an FST.
The IRSTLM build process requires automake, aclocal, and libtoolize (the corresponding packages are automake and libtool).
Note: some of the example scripts now use SRILM; we make it easy to install that, although you still have to register online to download it.
SRILM: some of the example scripts use this. It's generally a better and more complete language modeling toolkit than IRSTLM; the only drawback is the license, which is not free for commercial use. You have to enter your name on the download page to download it, so the installation script requires some human interaction.
sph2pipe: this is for converting sph format files into other formats such as wav. It's needed for the example scripts that use LDC data.
sclite: this is for scoring and is not necessary as we have our own, simple scoring program (compute-wer.cc).
ATLAS, the linear algebra library. This is only needed for the headers; in typical setups we expect that ATLAS will be on your system. However, if it not already on your system you can compile ATLAS as long as your machine does not have CPU throttling enabled.
CLAPACK, the linear algebra library (we download the headers). This is useful only on systems where you don't have ATLAS and are instead compiling with CLAPACK.
OpenBLAS: this is an alernative to ATLAS or CLAPACK. The scripts don't use it by default but we provide installation scripts so you can install it if you want to compare it against ATLAS (it's more actively maintained than ATLAS).


```
extras/check_dependencies.sh
extras/check_dependencies.sh: all OK.
wget -T 10 -t 3 http://www.openslr.org/resources/3/sph2pipe_v2.5.tar.gz || \
wget --no-check-certificate -T 10  https://sourceforge.net/projects/kaldi/files/sph2pipe_v2.5.tar.gz
--2017-08-26 15:36:42--  http://www.openslr.org/resources/3/sph2pipe_v2.5.tar.gz
Resolving www.openslr.org (www.openslr.org)... 35.184.122.207
Connecting to www.openslr.org (www.openslr.org)|35.184.122.207|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 329832 (322K) [application/x-gzip]
Saving to: ‘sph2pipe_v2.5.tar.gz’
```

Sphinx
---

https://cmusphinx.github.io/wiki/raspberrypi/


https://howchoo.com/g/ztbhyzfknze/how-to-install-pocketsphinx-on-a-raspberry-pi#download-the-latest-version-of-sphinxbase-and-pocketsphinx

```
pi@raspberrypi:~ $ cat /proc/asound/cards
 0 [ALSA           ]: bcm2835 - bcm2835 ALSA
                      bcm2835 ALSA
 1 [UAC20          ]: USB-Audio - ReSpeaker MicArray UAC2.0
                      SeeedStudio ReSpeaker MicArray UAC2.0 at usb-3f980000.usb-1.5, full speed
 2 [Device         ]: USB-Audio - USB PnP Sound Device
                      C-Media Electronics Inc. USB PnP Sound Device at usb-3f980000.usb-1.2, full spe
```

https://wolfpaulus.com/embedded/raspberrypi2-sr/

交换空间的问题

https://www.bitpi.co/2015/02/11/how-to-change-raspberry-pis-swapfile-size-on-rasbian/



中文模型

https://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/Mandarin/

