## Pyaudio 安装

## 1,安装依赖包portaudio

## 2,安装alsa-utils工具

​		alsa-utils，其中包 amixer, aplay, arecord 等命令

## 3.Pyaudio测试

​	录音测试

```python
import pyaudio
import wave

FORMAT = pyaudio.paInt16
CHANNEL = 1
RATE = 44100
CHUNK = 1024
RECORD_SEC = 5
OUT_FILE = 'demo.wav'

if __name__ == "__main__" :
    print("hello python")
    pa = pyaudio.PyAudio()
    stream = pa.open( format=FORMAT,
                        channels=CHANNEL,
                        rate=RATE,
                        input=True,
                        frames_per_buffer=CHUNK)
    print("*recording")

  

    frames = []
    for i in range(0,int(RATE/CHUNK*RECORD_SEC)):
        data = stream.read(CHUNK)
        frames.append(data)

    stream.stop_stream()
    stream.close()

    with wave.open(OUT_FILE,"wb") as wf:
        wf.setnchannels(CHANNEL)
        wf.setsampwidth(pa.get_sample_size(FORMAT))
        wf.setframerate(RATE)
        wf.writeframes(b''.join(frames))

    pa.terminate()
    print("*output %s"%(OUT_FILE))
```

```
aplay deme.wav 命令可以播放录音音频
```

​	音频播放

```python
import pyaudio
import wave
import time

if __name__ == "__main__" :
    pa = pyaudio.PyAudio()
    wf = wave.open("demo.wav", 'rb')

    def callback(in_data, frame_count, time_info, status):
        data = wf.readframes(frame_count)
        return (data, pyaudio.paContinue)

    stream = pa.open(format=pa.get_format_from_width(wf.getsampwidth()),
                    channels=wf.getnchannels(),
                    rate=wf.getframerate(),
                    output=True,
                    stream_callback=callback)

    stream.start_stream()

    while stream.is_active():
        time.sleep(0.1)

    stream.stop_stream()
    stream.close()
    wf.close()
```

