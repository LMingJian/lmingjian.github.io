---
title: github 凭证管理
date: 2023-08-11
author: LM
---

## 1.为什么需要凭证管理

在使用 git bash 拉取 Github 项目时，若用户未配置 SSH 登录，则系统会弹出登录窗口提醒你输入账号密码登录，用户只能在登录后提交或拉取内容。凭证管理就是为了解决每次提交或拉取都需要登录这个问题。

## 2.使用 Github CLI 进行凭证管理

根据 Github Docs [ 关于向 GitHub 验证 ](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/about-authentication-to-github) 的描述，用户使用 HTTPS 处理仓库时，基于密码的身份验证已被删除，因此用户无法通过保存的账号密码来提交和拉取项目。文档中推荐使用  GitHub CLI 进行身份验证。

```
HTTPS
即使您在防火墙或代理后面，也可以通过 HTTPS 处理 GitHub 上的所有仓库。

如果使用 GitHub CLI 进行身份验证，可以使用 personal access token 或通过 Web 浏览器进行身份验证。 有关使用 GitHub CLI 进行身份验证的详细信息，请参阅 gh auth login。

如果不使用 GitHub CLI 进行身份验证，则必须使用 personal access token 进行身份验证。 当 Git 提示你输入密码时，请输入你的personal access token。 或者，可以使用 Git 凭据管理器等凭据帮助程序。Git 的基于密码的身份验证已被删除，以支持更安全的身份验证方法。有关详细信息，请参阅“管理个人访问令牌”。除非你使用凭据小助手缓存了凭据，否则每次使用 Git 向 GitHub 验证时，系统都会提示你输入凭据以向 GitHub 验证。
```

从官网[ GitHub CLI ](https://cli.github.com/)中下载安装，安装完成后用户可以直接在 cmd 输入 `gh` 来检查命令。执行命令 `gh auth login`，开始身份验证。

回车，直到 `Login with a web browser` ，使用浏览器进行身份验证。

![](/images/drawingbed/img/202308111036924.png)

系统会给出一个 8 位验证码，用户需要在网页中填入该验证码，以进行身份验证。

![](/images/drawingbed/img/202308111039932.png)

![](/images/drawingbed/img/202308111050364.png)

点击 Continue 后再点击 Authorize github 以完成验证，此时可以在 Windows 的凭证管理器中查看到对应的凭证记录。

![](/images/drawingbed/img/202308111042616.png)

{{< details "参考文件" >}} 
1：[ github 账号凭据 @简书 ](https://www.jianshu.com/p/02048162b9b9)
{{< /details >}}