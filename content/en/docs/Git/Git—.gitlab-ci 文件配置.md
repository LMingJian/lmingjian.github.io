---
title: .gitlab-ci 文件配置
date: 2021-01-13
author: LM
---

## 1.gitlab-ci 文件

GitLab CI 使用 .gitlab-ci.yml 文件来管理项目配置，用来配置 CI 将要在你项目中完成的操作，这个文件位于仓库的根目录下。

当新内容 push 到仓库或者有代码合并后， GitLab 会查找是否有 .gitlab-ci.yml 文件，如果文件存在，Runners 将会根据该文件的内容开始执行构建。

## 2.示例

```yaml
# 定义 stages（阶段）。任务将按此顺序执行。
stages:
  - build
  - test
 
# 定义 job（任务）
job1:
  stage: test
  tags:
    - XX  # 只有标签为XX的runner才会执行这个任务
  only:
    - dev # 只有dev分支提交代码才会执行这个任务。也可以是分支名称或触发器名称
    - /^future-.*$/ # 正则表达式，只有future-开头的分支才会执行
  script:
    - echo "I am job1"
    - echo "I am in test stage"
    
job2:
  stage: test # 如果此处没有定义stage，其默认也是test
  only:
    - master # 只有master分支提交代码才会执行这个任务
  script:
    - echo "I am job2"
    - echo "I am in test stage"
  allow_failure: true # 允许失败，即不影响下步构建

.job3:
# 对于临时不想执行的job，可以选择在前面加个"."，这样就会跳过此步任务，否则你除了要注释掉这个job3外，还需要注释上面为build的stage
  stage: build
  except:
    - dev # 除了dev分支，其它分支提交代码都会执行这个任务
  script:
    - echo "I am job3"
    - echo "I am in build stage"
    
# 下面几个都相当于全局变量
before_script:
  - echo "每个job之前都会执行"
  - export MVN_HOME
  - export JAVA_HOME
  - java -version
  - sh /home/gitlab-runner/kill.sh

after_script:
  - echo "每个job之后都会执行"
  
variables: 
# 变量
  DATABASE_URL: "postgres://postgres@postgres/my_database" 
  # 在job中可以用${DATABASE_URL}来使用这个变量。常用的预定义变量有CI_COMMIT_REF_NAME（项目所在的分支或标签名称），CI_JOB_NAME（任务名称），CI_JOB_STAGE（任务阶段）

  GIT_STRATEGY: "none" 
  # GIT策略，定义拉取代码的方式，有3种：clone/fetch/none，默认为clone，速度最慢，每步job都会重新clone一次代码。我们一般将它设置为none，在具体任务里设置为fetch就可以满足需求，毕竟不是每步都需要新代码，那也不符合我们测试的流程
  
cache: 
# 缓存
#因为缓存为不同管道和任务间共享，可能会覆盖，所以有时需要设置key
  key: ${CI_COMMIT_REF_NAME} 
  # 启用每分支缓存。
  # key: "$CI_JOB_NAME/$CI_COMMIT_REF_NAME" # 启用每个任务和每个分支缓存。需要注意的是，如果是在windows中运行这个脚本，需要把$换成%

  untracked: true 
  # 缓存所有Git未跟踪的文件

  paths: 
  # 以下2个文件夹会被缓存起来，下次构建会解压出来
    - node_modules/
    - dist/
    
```

## 3.Stages

Stages 表示构建阶段，默认有3个 stages ： build，test，deploy 。我们可以在一次 Pipeline 中定义多个 Stages ，这些 Stages 有以下特点：

- 所有 Stages 会按照顺序运行，即当一个 Stage 完成后，下一个 Stage 才会开始
- 只有当所有 Stages 完成后，该构建任务 (Pipeline) 才会成功
- 如果任何一个 Stage 失败，那么后面的 Stages 不会执行，该构建任务 (Pipeline) 失败

## 4.Jobs

Jobs 表示构建工作，表示某个 Stage 里面执行的工作。我们可以在 Stages 里面定义多个 Jobs ，Jobs 是由 Runners 接管并且由服务器中 runner 执行。这些 Jobs 有以下特点：

- 每一个 Jobs 的执行过程都是独立运行的
- 相同 Stage 中的 Jobs 会并行执行
- 相同 Stage 中的 Jobs 都执行成功时，该 Stage 才会成功
- 如果任何一个 Job 失败，那么该 Stage 失败，即该构建任务 (Pipeline) 失败

## 5.约束

YAML 文件定义了一系列带有约束说明的任务。这些任务都是以任务名开始并且至少要包含 script 部分。

```yaml
job1:
  script: "execute-script-for-job1"
  
job2:
  script: "execute-script-for-job2"
```

## 6.跳过job

如果你的 commit 信息包涵 ci skip 或者 skip ci ，不论大小写，这个 commit 创建时，不会执行 job 任务。

## 7.开始

```yaml
before_script:
  - echo "每个job之前都会执行"
```

## 8.结束

```yaml
after_script:
  - echo "每个job之后都会执行"
```

## 9.保留字段

这些保留字段不能被定义为 job 名称。

- **image和services**：这两个关键字允许使用一个自定义的 Docker 镜像和一系列的服务，并且可以用于整个 job 周期
- **before_script**：before_script 用来定义所有 job 之前运行的命令
- **after_script**：after_script 用来定义所有 job 之后运行的命令
- **stages**：stages 用来定义可以被 job 调用的 stages。stages 的规范允许有灵活的多级 pipelines。stages 中的元素顺序决定了对应 job 的执行顺序
- **types**：与 stages 同义
- **variables**：变量
- **cache**：用来指定需要在 job 之间缓存的文件或目录。只能使用该项目工作空间内的路径。
- **jobs**：工作
- **script**：脚本

## 10.only and except

only 和 except 是两个参数用分支策略来限制jobs构建：

- only 定义哪些分支和标签的 git 项目将会被 job 执行。
- except 定义哪些分支和标签的 git 项目将不会被 job 执行。

下面是 refs 策略的使用规则：

- only 和 except 可同时使用。如果 only 和 except 在一个 job 配置中同时存在，则以 only 为准，跳过 except。
- only 和 except 可以使用正则表达式。
- only 和 except 允许使用特殊的关键字：branches，tags和triggers。
- only 和 except 允许使用指定仓库地址但不是 forks 的仓库。

## 11.tags

tags 可以从允许运行此项目的所有 Runners 中选择特定的 Runners 来执行 jobs。

在注册 Runner 的过程中，我们可以设置 Runner 的标签，比如 ruby，postgres，development。

tags 可通过 tags 来指定特殊的 Runners 来运行 jobs。

## 12.allow_failure

allow_failure 可以用于当你想设置一个 job 失败的之后并不影响后续的 CI 组件的时候。失败的 jobs 不会影响到 commit 状态。

当开启了允许 job 失败，所有的 intents 和 purposes 里的 pipeline 都是成功/绿色，但是也会有一个"CI build passed with warnings"信息显示在 merge request 或 commit 或 job page。这被允许失败的作业使用，但是如果失败表示其他地方应采取其他（手动）步骤。

## 13.when

when is used to implement jobs that are run in case of failure or despite the failure.

when 可以设置以下值：

- on_success - 只有前面 stages 的所有工作成功时才执行。 这是默认值。
- on_failure - 当前面 stages 中任意一个 jobs 失败后执行。
- always - 无论前面 stages 中 jobs 状态如何都执行。
- manual- 手动执行(GitLab8.10增加)。更多请查看手动操作。

## 14.Manual actions

手动操作指令是不自动执行的特殊类型的 job；它们必须要人为启动。手动操作指令可以从 pipeline，build，environment 和 deployment 视图中启动。

{{< details "参考文件" >}} 
1：[ .gitlab-ci.yml语法  @hellojinni  ](https://blog.51cto.com/7072753/2457095)
{{< /details >}}