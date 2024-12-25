---
title: 如何完成 Flutter 构建时的网络证书验证
date: 2024-12-25T11:20:40+08:00
author: LiangMingJian
---

# 背景

Flutter 在较新版本中构建项目时，会强制连接 Github，连接失败则无法进行构建。在国内，通常使用代理或加速器这些方式来访问 Github，这就导致了证书不能被项目识别，验证失败，此时报错：`javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target`

# 通过 Java 的 Keytool 来导入证书

在 Java 中可以使用工具 [SSLPoke](https://confluence.atlassian.com/kb/files/779355358/779355357/1/1441897666313/SSLPoke.class) 来测试连接是否可信，执行命令 `$JAVA_HOME/bin/java SSLPoke java.example.com 443`，如果连接成功，则会出现 `Successfully connected` 的提示。

通过 Keytool （和 java 命令在同一个文件夹内）可以管理或导入安全证书以供连接使用。

执行命令 `.\keytool -import -alias githubsteam -keystore ../lib/security/cacerts -file SteamTools.Certificate.cer` 导入证书。

通过 `.\keytool -list -keystore ../lib/security/cacerts` 检查证书。

{{< details "参考文件" >}} 
1：[ JAVA 导入信任证书 ( Keytool 的使用）@CSDN博客 ](https://blog.csdn.net/ljskr/article/details/84570573)
{{< /details >}}
