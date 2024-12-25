---
title: gitlab-runner 的安装配置
date: 2024-12-25T13:48:04+08:00
author: LiangMingJian
---

# 安装 gitlab-runner 

gitlab-runner 是 gitlab 提供的一个开源持续集成服务，它能与GitLab CI一起使用，在运行您的作业后，将结果发送回 GitLab。

gitlab-runner 的安装可以通过下载相应的安装脚本进行。

```bash
# Install GitLab Runner using the official GitLab repositories
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
export GITLAB_RUNNER_DISABLE_SKEL=true; sudo -E yum install gitlab-runner
# https://docs.gitlab.com/runner/install/linux-repository.html
```

# 注册 gitlab-runner

```bash
sudo gitlab-runner register
Runtime platform arch=amd64 os=linux pid=3639 revision=943fc252 version=13.7.0
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
# http://gitlab.example.net/
Enter the registration token:
# token
Enter a description for the runner:
# description
Enter the tags for the runner:
# tags
Enter the runner executor:
# shell
———————————————————————————————————————————————————————————————————————————————————————————————————————————
# 文档
sudo gitlab-runner register
Enter your GitLab instance URL (also known as the gitlab-ci coordinator URL).
token Enter the token you obtained to register the runner.
Enter a description for the runner. You can change this value later in the GitLab user interface.
Enter the tags associated with the runner, separated by commas. You can change this value later in the GitLab user interface.
Provide the runner executor. For most use cases, enter docker.
If you entered docker as your executor, you’ll be asked for the default image to be used for projects that do not define one in .gitlab-ci.yml.
```

# 支持的命令

```bash
# https://docs.gitlab.com/runner/commands/README.html
gitlab-runner --debug <command>
sudo gitlab-runner run
gitlab-runner install
gitlab-runner uninstall
gitlab-runner start # 后台运行
gitlab-runner stop
gitlab-runner restart
gitlab-runner status
```

# 运行

在启动后，可以通过 gitlab-runner 在 Linux 系统上运行脚本，编写 .gitlab-ci.yml 文件来布置任务。
