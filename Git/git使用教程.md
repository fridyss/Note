# GIT使用教程

​	本文主要参考 [git官方文档](https://git-scm.com/book/zh/v2)进行实验记录，github 项目使用测试。详情情况可以参官方文档。

## 1 关于版本控制

### 1.1 本地控制

​		本地控制系统（Local Control System）人为通过复制整个项目目录来进行版本管理，或许会添加日期和版本号加于区分各个版本的信息，这样一定程度上方便快捷，不需要花时间学习额外的应用，但是特别容易犯错 ，不可控因素太多。			

 			<img src="./image/LCS.jpg" alt="LCS" style="zoom:67%;" />

### 1.2 集中控制

​		本地控制系统不具备多人协同开发的功能，集中版本控制系统（Centralized Version Control System）可以很好地解决这问题。CVCS就是有一个集成服务器来保存所有修定版本。每个开发人员，可以通过客户端进行远程拉取和提交更新操作。缺点也很明显，必须要有一个中央服务器来存储版本信息，一但没有了网络就无法进行协同工作了。

<img src="./image/CVCS.jpg" alt="CVCS.jpg" style="zoom:50%;" />

### 1.3 分布式控制

​		分布式版本控制系统（Distributed Version Control System，简称 DVCS ),  客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

<img src="./image/DVCS.jpg" alt="DVCS" style="zoom:50%;" />

## 2 Git简介

​		git 与其他版本控制（SVN）最大的区别就是，**直接记录快照，并非保存差异**。每当你提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照并保存这个快照的索引。为了效率，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 快照流。

### 2.1 基本概念

​	git 项目分三个阶段， Workspace，暂存区（index,staged），Repository。在没有远程仓库时，三大区也能独立运行。

- Workspace： 工作区，平时用于存放项目目录的地方。
- Index/staged:   暂存区，用于临时存放你的改动。
- Repository：仓库区（.git目录），所有版本的数据都存放在这。HEAD指向最新放入仓库的版本。

- Remote: 远程仓库，托管代码的服务器。

### 2.2 工作流程

​		git基本流程工作由，clone, add, commit ,push, pull , fetch 命令组成。这些命令操作文件，会让文件产生四种状态，分别为

Untracked, Unmodify, Modify, Stated。

<img src="./image/work.jpg" alt="工作流程" style="zoom: 67%;" />

- Untracked ：未跟踪，表示文件处于项目目录，但未受版本系统控制， 通过`git add`命令可以将其状态改为Stated。
- Unmodify： 受版本系统控制， 且和当前版本内容一致。如果被修改，则状态变为modify,可以通过命令`git rm`移出版本库变为untracked状态。

- Modify：文件已经修改，通过`git add` 命令，将其提交的暂存区，状态改为Stated,  可能通过`git checkout`命令，检出当前版本的文件将其覆盖。状态改为Unmodify。
- Stated： 暂存状态，通过`git commit`命令提交到版本库，状态改为Unmmodify,  执行`git reset HEAD filename`取消暂存, 文件状态为Modified。

<img src="./image/status.jpg" alt="status" style="zoom:67%;" />

## 3 Git安装与配置

- ubuntu下安装git

```bash
sudo apt install git-all
```

- 配置提交代码的用户与邮箱

```bash
$ git config --global user.name "fridy"
$ git config --global user.email fridy@outlook.com
$ git config --list 			# 可以查看配置情况
user.email=fridy@outlook.com
user.name=fridy
```

- 生成ssh密钥

```bash
$ ssk-keygen -t ed25519 -f filename #-t 加密方式, -f  输出文件名
```

- 更改gitk gui 的编码方式

```bash
$ git config --global gui.encoding utf-8 # gitk 和 gui 命令会打界面git,不设置会出现中文乱码。
```

## 3 Git仓库



## 4 Git基本操作

​		Git 基本操作命令，可以分为

## 5 Git分支管理



## 5 Git原理



