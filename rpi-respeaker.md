
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

