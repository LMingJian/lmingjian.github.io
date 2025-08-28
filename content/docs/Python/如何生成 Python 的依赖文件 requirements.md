---
title: 如何生成 Python 的依赖文件 requirements
date: 2024-12-25T16:08:57+08:00
author: LiangMingJian
---

# 需求

在移植项目时，通过需要 requirements 文件来安装所有的依赖库。

# 使用

```bash
# 安装模块
pip install -r requirements.txt

# 卸载所有模块
pip uninstall -r requirements.txt -y
```

# 通过 pip 生成

`freeze` 会导出项目的所有模块，包括模块的间接依赖。

比如常用的网络请求模块 `requests`，其间接依赖包括： `certifi`，`charset-normalizer`，`idna`，`urllib3`。

```bash
pip freeze > requirements.txt
```

# 通过 pipreqs 生成

和 `freeze` 不同， `pipreqs` 会分析指定目录里面所有 Python 文件的 import 语句，只导出模块本体，不会导出相应的间接依赖。

```bash
# 安装
pip install pipreqs

# 使用
pipreqs ./ --encoding=utf8 --force --ignore=.venv
```

- `./`：指定当前目录的 Python 文件进行分析。
- `--encoding=utf8`：指定编码为 UTF-8。
- `--force`：如果已经存在 `requirements.txt` 则强制覆盖。
- `--ignore=.venv`：忽略指定文件夹。注意，在具有虚拟环境的项目中，必须忽略 `.venv` 文件夹，否则在使用 `pipreqs` 导出时，程序会分析 `.venv` 文件里面的 Python 文件，从而造成编码错误等问题。

推荐在 `pipreqs` 导出后，自行检查模块是否被完整导出，避免因为分析错误导致的模块缺漏。

