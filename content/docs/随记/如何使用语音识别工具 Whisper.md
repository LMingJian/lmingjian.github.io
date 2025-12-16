---
title: 如何使用语音识别工具 Whisper
date: 2024-12-25T10:30:52+08:00
author: LiangMingJian
---

# Whisper 是什么

Whisper 是 OpenAI 开源的一个语音识别项目，项目地址位于：[ Whisper Github ](https://github.com/openai/whisper)。

# Whisper 的运行环境

Whisper 需要电脑拥有以下环境：

```
python 3.9.9+
pip 24.0+
ffmpeg
```

# Whisper 的模型

Whisper 目前主要推出了 tiny、base、small、medium、larg 这几类大模型，在 Whisper 运行时，这些模型会自动下载。

但是需要注意，由于国内网络问题，这些模型下载可能会很慢，容易网络异常，因此建议前往对应的模型地址，手动下载。

各种模型的下载地址位于：[ Whisper init ](https://github.com/openai/whisper/blob/main/whisper/__init__.py)

![](_images/drawingbed/img/Pasted%20image%2020251208155123.png)

# Whisper 的安装使用

若电脑满足上述环境，则执行以下命令行进行安装。

```python
pip install -U openai-whisper
```

安装成功后，可以在命令行中执行以下命令来检查运行，或进行 AI 识别。

```python
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
