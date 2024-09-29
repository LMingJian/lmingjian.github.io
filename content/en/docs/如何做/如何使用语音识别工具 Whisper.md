---
title: 如何使用语音识别工具 Whisper
date: 2024-08-26
author: LM
---

## 1.Whisper 的使用

Whisper 是 OpenAI 开源的一个语音识别项目，项目地址为：https://github.com/openai/whisper，目前主要推出了 tiny、base、small、medium、larg 这几类大模型，用户可以按需进行下载使用。模型地址位于：https://github.com/openai/whisper/blob/main/whisper/__init__.py，用户可手动进行下载使用。

安装 Whisper 需要电脑拥有以下环境。

```
python 3.9.9+
pip 24.0+
ffmpeg
```

若电脑以满足上述环境，则执行以下命令行进行安装。

```
pip install -U openai-whisper
```

安装成功后，可以在命令行中执行以下命令来检查运行，或进行 AI 识别。

```
whisper --help
whisper audio.mp3 --option
whisper japanese.wav --language Japanese --model medium
```

常用的 option 参数有。

```python
--model # 指定使用模型，默认使用 --model small
--language # 指定转录语言，比如指定中文 --language Chinese
--device # 指定硬件加速，默认使用 auto 自动选择，--device cuda 使用显卡，--device cpu 使用 CPU，注意使用显卡时，电脑需要安装 CUDA
```

