---
title: py 文件的头部信息
date: 2024-12-25T16:11:01+08:00
author: LiangMingJian
---

# 概述

在 Python 的代码 .py 文件中，支持设置诸如以下内容的头部信息，用来标注代码的执行环境和作者等基础信息。

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : ${DATE} ${TIME}
# @Author  : name
# @File    : ${NAME}.py
```

# #!/usr/bin/env python

在 Linux 系统下，操作系统会根据文件头（首行）的标记来判断文件类型，然后通过文件所指定的程序来运行。

`#!/usr/bin/env python` 是告诉系统需要先到 env 设置里查找 Python 的安装路径，然后再去调用对应路径下的解释器程序完成操作。这样写的目的是避免用户在安装 Python 时没有将 Python 安装在默认的` /usr/bin` 中。

在文件头中，也可以直接写入 `#!/usr/bin/python` 来指定 Python 路径。

在 Windows 系统中，由于操作系统是根据文件名的后缀（扩展名）来判断文件类型的，因此，.py 文件的文件头 `#!/usr/bin/python` 或 `#!/usr/bin/env python` 在 Windows 系统下相当于普通的注释，并没有意义。

# -\*- coding: utf-8 -\*-

在 Python 2.x 版本中，.py 文件一般默认的是 ASCII 码，如果文件里有中文时，运行会出现乱码，即使只是注释是中文也不行。因此，需要在文件头信息中写入 `# -*- coding:utf-8 -*-` 来把文件编码改为 utf-8。

在 Python 3.x 版本中，.py 文件的默认编码已经被改为 Unicode，也就是说，已经不用再进行编码声明，可以直接使用中文了。

# @Time、@Author、@File

这是 Pycharm 中支持设置的文件头信息，用来标注文件的创建时间，作者，文件名。

在 Pycharm 菜单 文件（File）->设置（Setting）->编辑器（Editor）->文件和代码模板（File and Code Templates）->Python Script 中，可以找到以上内容修改。
