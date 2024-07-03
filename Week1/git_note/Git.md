# git命令学习工具

GIT 是目前实际使用中最常见的**分布式版本控制系统**。没有中央服务器，每个人在自己的电脑中有项目的完整版本库，工作时不需要链接网络，只需要在个人修改后，将修改推送到协作者的中，就可以看到相互的修改。

## GIT工作流程介绍

![知乎图片链接失败，请检查网络](https://pic2.zhimg.com/80/v2-3bc9d5f2c49a713c776e69676d7d56c5_1440w.jpg)

上图展示了git的工作流程：git将项目划分为 工作区(workspace)、暂存区(stage)、本地仓库(repository)、远程仓库(remote).

其中各个分区具体功能以及总工作流程如下：

- **工作区**用作在指定的branch分支上，进行文件的修改（创建、删除、修改内容）操作。
- **暂存区**用于暂存在工作区进度，例如在工作区创建了文件或者添加了内容，使用`git add <file>`来将工作区中的修改加入暂存区。
- **本地仓库**用于保存本地工作的进度，在工作区的修改暂存到暂存区之后，使用`git commit -m "message"`将暂存区的进展按照message消息表示提交到本地仓库中。使用`git checkout`来对比**工作区**与**本地仓库**之间的区别。
- **远程仓库**用于将仓库中的内容存储在网络中，方便协作者进行拉取同步。在本地上，一项工作做完后，使用`git commit -m "message"`提交在本地仓库后，使用`git push <remote-name> <local-branch>`将本地的仓库内容推送到远程仓库中。此时协作者可以使用`git clone`或`git pull`命令进行拉取

## GIT的使用方法

在使用GIT之前，首先在本地上下载并配置git命令行工具。
在[git官网](https://git-scm.com/download)上按照对应的系统进行下载.

本电脑已经下载过，使用`git --version`查看git版本。

![file not found](image.png)

下载过后，使用全局方式配置git：

```shell
git config --global user.name "zip95297"
git config --global user.email "zip95297@gmail.com"
```

在配置时可以选择``--system``或者``--local``从系统级别或者本地一个指定的仓库进行配置。

### *本地* GIT代码管理

在对git进行了简单的配置之后，在本地建立一个仓库学习本地使用。以当前“PreLearn”为例子，建立一个仓库。首先在当前仓库**根目录**下，使用`git init`初始化本地仓库。在建立了本地仓库之后，可以发现在项目的根目录下，出现了一个名为.git的文件夹：
![file not found](image-1.png)

该.git的文件夹中即包括了暂存区和本地仓库的部分，若在本地想要删除git仓库，保留原文件，可以使用`rm -rf ./.git`

在建立了本地的git仓库后，在工作区中进行工作，然后使用git status检查工作区状态（）
![file not found](image-2.png)

### *远程* GIT代码管理