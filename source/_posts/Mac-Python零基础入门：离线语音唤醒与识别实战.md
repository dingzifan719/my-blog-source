---
title: Mac+Python零基础入门：离线语音唤醒与识别实战
date: 2025-08-24 22:26:43
tags:
--- 
# 准备工作：环境搭建

1. 打开终端，创建并激活一个全新的conda环境
```
(base) dddd-de-mac:Desktop dzf$ conda create -n voice-ai python=3.10
Collecting package metadata (current_repodata.json): done
Solving environment: done
==> WARNING: A newer version of conda exists. <==
  current version: 4.10.3
  latest version: 25.7.0
Please update conda by running
    $ conda update -n base -c defaults conda
## Package Plan ##
  environment location: /Users/dzf/opt/anaconda3/envs/voice-ai
  added / updated specs:
    - python=3.10
The following NEW packages will be INSTALLED:
  bzip2              pkgs/main/osx-64::bzip2-1.0.8-h6c40b1e_6
  ca-certificates    pkgs/main/osx-64::ca-certificates-2025.7.15-hecd8cb5_0
  expat              pkgs/main/osx-64::expat-2.7.1-h6d0c2b6_0
  libcxx             pkgs/main/osx-64::libcxx-19.1.7-haebbb44_3
  libffi             pkgs/main/osx-64::libffi-3.4.4-hecd8cb5_1
  ncurses            pkgs/main/osx-64::ncurses-6.5-h923df54_0
  openssl            pkgs/main/osx-64::openssl-3.0.17-hee2dfae_0
  pip                pkgs/main/noarch::pip-25.1-pyhc872135_2
  python             pkgs/main/osx-64::python-3.10.18-hc958d9f_0
  readline           pkgs/main/osx-64::readline-8.3-h49f2429_0
  setuptools         pkgs/main/osx-64::setuptools-78.1.1-py310hecd8cb5_0
  sqlite             pkgs/main/osx-64::sqlite-3.50.2-hc8b0dd6_1
  tk                 pkgs/main/osx-64::tk-8.6.15-h3a5a201_0
  tzdata             pkgs/main/noarch::tzdata-2025b-h04d1e81_0
  wheel              pkgs/main/osx-64::wheel-0.45.1-py310hecd8cb5_0
  xz                 pkgs/main/osx-64::xz-5.6.4-h46256e1_1
  zlib               pkgs/main/osx-64::zlib-1.2.13-h4b97444_1
Proceed ([y]/n)? y
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```

2. 激活conda环境
```
(base) dddd-de-mac:Desktop dzf$ conda activate voice-ai
```
之后会进入到(voice-ai)的命令行显示

3. 按照核心python库

```
# 1. 语音识别（核心工具）
pip install openai-whisper
# 2. 音频操作库
pip install sounddevice
# 3. 用Conda安装pyaudio，避免编译错误
conda install pyaudio
# 4. 语音合成（简单在线版，先快速实现效果）
pip install edge-tts
```
具体回显如下：
```
(voice-ai) dddd-de-mac:Desktop dzf$ conda install -c conda-forge edge-tts
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Collecting package metadata (repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.

PackagesNotFoundError: The following packages are not available from current channels:
  - edge-tts

Current channels:
  - https://conda.anaconda.org/conda-forge/osx-64
  - https://conda.anaconda.org/conda-forge/noarch
  - https://repo.anaconda.com/pkgs/main/osx-64
  - https://repo.anaconda.com/pkgs/main/noarch
  - https://repo.anaconda.com/pkgs/r/osx-64
  - https://repo.anaconda.com/pkgs/r/noarch
To search for alternate channels that may provide the conda package you're
looking for, navigate to
    https://anaconda.org
and use the search bar at the top of the page.


(voice-ai) dddd-de-mac:Desktop dzf$ pip install openai-whisper
Collecting openai-whisper
  Using cached openai_whisper-20250625-py3-none-any.whl
Collecting more-itertools (from openai-whisper)
  Using cached more_itertools-10.7.0-py3-none-any.whl.metadata (37 kB)
Collecting numba (from openai-whisper)
  Using cached numba-0.61.2-cp310-cp310-macosx_10_14_x86_64.whl.metadata (2.8 kB)
Collecting numpy (from openai-whisper)
  Using cached numpy-2.2.6-cp310-cp310-macosx_14_0_x86_64.whl.metadata (62 kB)
Collecting tiktoken (from openai-whisper)
  Using cached tiktoken-0.11.0-cp310-cp310-macosx_10_12_x86_64.whl.metadata (6.7 kB)
Collecting torch (from openai-whisper)
  Using cached torch-2.2.2-cp310-none-macosx_10_9_x86_64.whl.metadata (25 kB)
Collecting tqdm (from openai-whisper)
  Using cached tqdm-4.67.1-py3-none-any.whl.metadata (57 kB)
Collecting llvmlite<0.45,>=0.44.0dev0 (from numba->openai-whisper)
  Using cached llvmlite-0.44.0-cp310-cp310-macosx_10_14_x86_64.whl.metadata (4.8 kB)
Collecting regex>=2022.1.18 (from tiktoken->openai-whisper)
  Using cached regex-2025.7.34-cp310-cp310-macosx_10_9_x86_64.whl.metadata (40 kB)
Collecting requests>=2.26.0 (from tiktoken->openai-whisper)
  Using cached requests-2.32.5-py3-none-any.whl.metadata (4.9 kB)
Collecting charset_normalizer<4,>=2 (from requests>=2.26.0->tiktoken->openai-whisper)
  Using cached charset_normalizer-3.4.3-cp310-cp310-macosx_10_9_universal2.whl.metadata (36 kB)
Collecting idna<4,>=2.5 (from requests>=2.26.0->tiktoken->openai-whisper)
  Using cached idna-3.10-py3-none-any.whl.metadata (10 kB)
Collecting urllib3<3,>=1.21.1 (from requests>=2.26.0->tiktoken->openai-whisper)
  Using cached urllib3-2.5.0-py3-none-any.whl.metadata (6.5 kB)
Collecting certifi>=2017.4.17 (from requests>=2.26.0->tiktoken->openai-whisper)
  Using cached certifi-2025.8.3-py3-none-any.whl.metadata (2.4 kB)
Collecting filelock (from torch->openai-whisper)
  Using cached filelock-3.19.1-py3-none-any.whl.metadata (2.1 kB)
Collecting typing-extensions>=4.8.0 (from torch->openai-whisper)
  Using cached typing_extensions-4.14.1-py3-none-any.whl.metadata (3.0 kB)
Collecting sympy (from torch->openai-whisper)
  Using cached sympy-1.14.0-py3-none-any.whl.metadata (12 kB)
Collecting networkx (from torch->openai-whisper)
  Using cached networkx-3.4.2-py3-none-any.whl.metadata (6.3 kB)
Collecting jinja2 (from torch->openai-whisper)
  Using cached jinja2-3.1.6-py3-none-any.whl.metadata (2.9 kB)
Collecting fsspec (from torch->openai-whisper)
  Using cached fsspec-2025.7.0-py3-none-any.whl.metadata (12 kB)
Collecting MarkupSafe>=2.0 (from jinja2->torch->openai-whisper)
  Using cached MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl.metadata (4.0 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy->torch->openai-whisper)
  Using cached mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
Using cached more_itertools-10.7.0-py3-none-any.whl (65 kB)
Using cached numba-0.61.2-cp310-cp310-macosx_10_14_x86_64.whl (2.8 MB)
Using cached llvmlite-0.44.0-cp310-cp310-macosx_10_14_x86_64.whl (28.1 MB)
Using cached numpy-2.2.6-cp310-cp310-macosx_14_0_x86_64.whl (6.9 MB)
Using cached tiktoken-0.11.0-cp310-cp310-macosx_10_12_x86_64.whl (1.1 MB)
Using cached regex-2025.7.34-cp310-cp310-macosx_10_9_x86_64.whl (289 kB)
Using cached requests-2.32.5-py3-none-any.whl (64 kB)
Using cached charset_normalizer-3.4.3-cp310-cp310-macosx_10_9_universal2.whl (207 kB)
Using cached idna-3.10-py3-none-any.whl (70 kB)
Using cached urllib3-2.5.0-py3-none-any.whl (129 kB)
Using cached certifi-2025.8.3-py3-none-any.whl (161 kB)
Using cached torch-2.2.2-cp310-none-macosx_10_9_x86_64.whl (150.8 MB)
Using cached typing_extensions-4.14.1-py3-none-any.whl (43 kB)
Using cached filelock-3.19.1-py3-none-any.whl (15 kB)
Using cached fsspec-2025.7.0-py3-none-any.whl (199 kB)
Using cached jinja2-3.1.6-py3-none-any.whl (134 kB)
Using cached MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl (14 kB)
Using cached networkx-3.4.2-py3-none-any.whl (1.7 MB)
Using cached sympy-1.14.0-py3-none-any.whl (6.3 MB)
Using cached mpmath-1.3.0-py3-none-any.whl (536 kB)
Using cached tqdm-4.67.1-py3-none-any.whl (78 kB)
Installing collected packages: mpmath, urllib3, typing-extensions, tqdm, sympy, regex, numpy, networkx, more-itertools, MarkupSafe, llvmlite, idna, fsspec, filelock, charset_normalizer, certifi, requests, numba, jinja2, torch, tiktoken, openai-whisper
Successfully installed MarkupSafe-3.0.2 certifi-2025.8.3 charset_normalizer-3.4.3 filelock-3.19.1 fsspec-2025.7.0 idna-3.10 jinja2-3.1.6 llvmlite-0.44.0 more-itertools-10.7.0 mpmath-1.3.0 networkx-3.4.2 numba-0.61.2 numpy-2.2.6 openai-whisper-20250625 regex-2025.7.34 requests-2.32.5 sympy-1.14.0 tiktoken-0.11.0 torch-2.2.2 tqdm-4.67.1 typing-extensions-4.14.1 urllib3-2.5.0
(voice-ai) dddd-de-mac:Desktop dzf$ pip install sounddevice
Collecting sounddevice
  Using cached sounddevice-0.5.2-py3-none-macosx_10_6_x86_64.macosx_10_6_universal2.whl.metadata (1.6 kB)
Collecting CFFI>=1.0 (from sounddevice)
  Using cached cffi-1.17.1-cp310-cp310-macosx_10_9_x86_64.whl.metadata (1.5 kB)
Collecting pycparser (from CFFI>=1.0->sounddevice)
  Using cached pycparser-2.22-py3-none-any.whl.metadata (943 bytes)
Using cached sounddevice-0.5.2-py3-none-macosx_10_6_x86_64.macosx_10_6_universal2.whl (108 kB)
Using cached cffi-1.17.1-cp310-cp310-macosx_10_9_x86_64.whl (182 kB)
Using cached pycparser-2.22-py3-none-any.whl (117 kB)
Installing collected packages: pycparser, CFFI, sounddevice
Successfully installed CFFI-1.17.1 pycparser-2.22 sounddevice-0.5.2
(voice-ai) dddd-de-mac:Desktop dzf$ conda install pyaudio
Collecting package metadata (current_repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.10.3
  latest version: 25.7.0

Please update conda by running

    $ conda update -n base -c defaults conda



## Package Plan ##

  environment location: /Users/dzf/opt/anaconda3/envs/voice-ai

  added / updated specs:
    - pyaudio


The following NEW packages will be INSTALLED:

  portaudio          pkgs/main/osx-64::portaudio-19.6.0-h6d0c2b6_5
  pyaudio            pkgs/main/osx-64::pyaudio-0.2.11-py310hca72f7f_2


Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
(voice-ai) dddd-de-mac:Desktop dzf$ pip install pvporcupine
Collecting pvporcupine
  Using cached pvporcupine-3.0.5-py3-none-any.whl.metadata (5.0 kB)
Using cached pvporcupine-3.0.5-py3-none-any.whl (2.5 MB)
Installing collected packages: pvporcupine
Successfully installed pvporcupine-3.0.5
(voice-ai) dddd-de-mac:Desktop dzf$ pwd
/Users/dzf/Desktop
(voice-ai) dddd-de-mac:Desktop dzf$ mkdir tts-demo
(voice-ai) dddd-de-mac:Desktop dzf$ cd tts-demo/
(voice-ai) dddd-de-mac:tts-demo dzf$ touch wake_word_demo.py
(voice-ai) dddd-de-mac:tts-demo dzf$ python wake_word_demo.py
Traceback (most recent call last):
  File "/Users/dzf/Desktop/tts-demo/wake_word_demo.py", line 2, in <module>
    from pvrecorder import PvRecorder
ModuleNotFoundError: No module named 'pvrecorder'
(voice-ai) dddd-de-mac:tts-demo dzf$ pip install pvporcupine pvrecorder
Requirement already satisfied: pvporcupine in /Users/dzf/opt/anaconda3/envs/voice-ai/lib/python3.10/site-packages (3.0.5)
Collecting pvrecorder
  Downloading pvrecorder-1.2.7-py3-none-any.whl.metadata (2.0 kB)
Downloading pvrecorder-1.2.7-py3-none-any.whl (4.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.1/4.1 MB 7.5 kB/s eta 0:00:00
Installing collected packages: pvrecorder
Successfully installed pvrecorder-1.2.7
```
# 步骤一：语音唤醒 (Wake Word)
目标：让程序能一直监听，并在听到特定词（如“小电脑”）时激活。

1. 去 [Picovoice官网](https://console.picovoice.ai/) 注册账号。
2. 在控制台创建一个免费的唤醒词（比如“小电脑”的中文模型），下载 .ppn 文件。
   注册完成后点击“Create Wake Word”（创建唤醒词）按钮，即可得到最后的.ppn文件。

3. 安装库：pip install pvporcupine

4. 参考官方Python demo（如下），写一个循环监听麦克风的脚本。当检测到唤醒词时，打印“Wake Word Detected!”.
```
import pvporcupine
from pvrecorder import PvRecorder
import wave
import struct

# 1. 设置你的参数
access_key = "daBa/uoDwP9huDOibGrUOqCcyEz1+xM/vWgEpaN6+PFnjLOiknr9UA=="  # 从Picovoice控制台获取
keyword_path = "小电脑_zh_mac_v3_0_0.ppn"  # 例如：xiao-dian-nao_zh_mac_v3_0_0.ppn
model_path = "porcupine_params_zh.pv"              # 你新下载的中文主模型文件

# 2. 初始化Porcupine
try:
    handle = pvporcupine.create(
        access_key=access_key,
        keyword_paths=[keyword_path],
        model_path=model_path  # 新增这一行，指定中文主模型
    )
except pvporcupine.PorcupineInvalidArgumentError as e:
    # 处理AccessKey错误
    print("AccessKey 错误: ", e)
    exit()
except pvporcupine.PorcupineActivationError as e:
    # 处理激活失败
    print("激活失败: ", e)
    exit()

# 获取Porcupine所需的音频参数
porcupine_frame_length = handle.frame_length

# 3. 初始化录音器
recorder = PvRecorder(
    frame_length=porcupine_frame_length,
    device_index=-1  # -1 表示使用默认麦克风
)
recorder.start()

print("开始监听... 请说唤醒词『小电脑』")
print("按 Ctrl+C 停止程序")

try:
    while True:
        # 从麦克风读取一帧音频数据
        pcm = recorder.read()
        
        # 交给Porcupine处理这一帧
        keyword_index = handle.process(pcm)
        
        # 如果检测到唤醒词，keyword_index 会变为 0
        if keyword_index >= 0:
            print("\nWake Word Detected! 唤醒词 detected!")
            # 在这里添加你的下一步代码，比如开始录音等
            # break  # 如果你想检测一次就退出循环，可以取消这行的注释
            
except KeyboardInterrupt:
    print("正在停止...")
finally:
    # 4. 清理资源
    recorder.delete()
    handle.delete()
```
命令行的运行结果如下图：
![运行结果](/images/语音唤醒示意图.png)

# 步骤二：录音与语音识别 (STT)
目标：唤醒后，录制接下来几秒的音频，并转换成文字。

怎么做：使用Whisper。

在唤醒词检测到的回调函数里，使用 sounddevice 或 pyaudio 录制3-5秒的音频，保存为 command.wav。

写一个函数，调用Whisper的 tiny 或 base 模型来识别 command.wav：
```
import whisper
model = whisper.load_model("base")
result = model.transcribe("command.wav", language='chinese')
print(result["text"])
```


# 步骤三：处理与回应 (TTS)
目标：对识别出的文字做出简单回应，并合成为语音播放。

怎么做：使用Edge-TTS。

写一个简单的逻辑处理函数：
```
def process_command(text):
    if "几点" in text:
        return f"现在是{datetime.datetime.now().strftime('%H:%M')}"
    elif "你好" in text:
        return "你好，主人！"
    else:
        return "我没听清，请再说一遍。"
```
将回应的文字传递给Edge-TTS：

```
# 直接在终端测试，会生成一个回应音频文件
edge-tts --text "你好，世界" --write-output response.wav
在Python中用 sounddevice 播放生成的 response.wav 文件。
```
