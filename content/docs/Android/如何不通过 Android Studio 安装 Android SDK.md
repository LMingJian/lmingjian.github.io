---
title: 如何不通过 Android Studio 安装 Android SDK
date: 2024-12-25T11:00:34+08:00
author: LiangMingJian
---

# 需求

在一般情况下，安装 Android SDK 的教程往往要求先下载 Android Studio，然后再进行安装。但某些时候，用户可能不希望安装 Android Studio，只想要 Android SDK，然后通过其他 IDE，如 VS Code 来进行编程。此时，如何不通过 Android Studio 安装 Android SDK 就是一个需求。

为了不通过 Android Studio 安装 Android SDK，我们需要使用到 Android SDK 的命令行管理工具 **sdkmanager** 进行手动下载。

# 下载并准备 sdkmanager

访问 [ Android Studio 下载页面 ](https://developer.android.google.cn/studio?hl=zh-cn)，把页面往下滚动，然后找到行——**仅限命令行工具**，选择目标平台的 SDK 工具包**下载**。

![](_images/drawingbed/img/Pasted%20image%2020251229142104.png)

**创建一个安装文件夹**，名称可以是 `F:\Sdk\android`，然后将压缩文件移动到这个文件夹内，**解压**。

![](_images/drawingbed/img/Pasted%20image%2020251229143342.png)

**注意！！！下面的步骤是重点，请务必跟随完成**。

首先，进入 `cmdline-tools` 文件夹，创建一个 `latest` 的子目录，然后将 `cmdline-tools` 文件夹内原有的全部文件和文件夹都移动到这个 `latest` 子目录中。

![](_images/drawingbed/img/Pasted%20image%2020251229145032.png)

此时，文件结构应该如下：

![](_images/drawingbed/img/Pasted%20image%2020251229145139.png)

接着，我们需要设置以下两个环境变量：

- 在 PATH 中，添加变量：`F:\Sdk\android\cmdline-tools\latest\bin`，即 sdkmanager 所在的地址。
- 添加一个新的环境键值对：`ANDROID_HOME=F:\Sdk\android`，指示 cmdline-tools 所在文件夹的地址，用来充当 Android SDK 的下载地址。

![](_images/drawingbed/img/Pasted%20image%2020251229145907.png)

> 在 Windows 中，你可以通过 `Windows + S` 调起搜索窗口，搜索 `环境变量`，一键跳转到环境变量设置页。

![](_images/drawingbed/img/Pasted%20image%2020251229150039.png)

在完成上述后，我们便可以在终端或 CMD 执行 `sdkmanager --version`，`sdkmanager --list` 等命令了。

![](_images/drawingbed/img/Pasted%20image%2020251229150422.png)

> 如果不完成上述配置，那么在运行 `sdkmanager --list` 这些命令时，程序就会出现报错。

```python
Error: Could not determine SDK root.
Error: Either specify it explicitly with --sdk_root= or move this package into its expected location: <sdk>\cmdline-tools\latest\
```

![](_images/drawingbed/img/Pasted%20image%2020251229145616.png)

# 安装 Android SDK

当你需要安装 Android SDK 时，你只需要在终端中执行以下命令即可：

```python
sdkmanager "platform-tools" "platforms;android-28" "build-tools;28.0.0"
```

> 安装过程中，会要求确认证书，输入 `Y` 即可。

![](_images/drawingbed/img/Pasted%20image%2020251229151146.png)

在这个命令中，分别安装了 Android SDK 的三个核心组件：

- **platform-tools**：包含 **adb** 在内的与 Android 设备交互的核心工具。
- **platforms;android-28**：适用 Android 9.0 的系统类库和框架文件。注意，如果需要编译的 Android 版本要求更高，则应当调整版本号。
- **build-tools;28.0.0**：适用于 Android 9.0 的编译工具。注意，如果需要编译的 Android 版本要求更高，则应当调整版本号。

等待下载完成后，Android SDK 即配置成功。

![](_images/drawingbed/img/Pasted%20image%2020251229151205.png)

访问 `ANDROID_HOME` 即 `F:\Sdk\android`，可以看见安装的内容。

![](_images/drawingbed/img/Pasted%20image%2020251229151456.png)

# 拓展阅读：什么是 sdkmanager

`sdkmanager` 是一个命令行工具，用户可以用它来查看、安装、更新和卸载 Android SDK 的软件包，而不用下载 Android Studio。

```bash
# 安装软件包
sdkmanager packages
sdkmanager "platforms;android-36" "build-tools;36.0.0"
# 卸载软件包
sdkmanager --uninstall packages
# 列出已安装和可用的软件包
sdkmanager --list
```

> [ sdkmanager @Android Studio 用户指南 ](https://developer.android.google.cn/tools/sdkmanager?hl=zh-cn)
