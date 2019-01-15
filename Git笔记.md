# Git笔记

> 分布式版本控制系统

## 安装

输入`git`检查系统有没有安装Git

如果没有安装，有很多Linux会友好地告诉你Git没有安装，还会告诉你如何安装Git。

安装命令: `sudo apt install git`

安装完成之后需要设置GitHub上的用户名和邮箱:

`git config --global user.name "Your Name"`

`git config --global user.email "email@example.com"`

`--global`这个参数表示这台机器上所有的Git库都会使用这个配置

##  创建本地版本库

选择合适的地方，创建一个空目录：

`mkdir learngit`

`cd learngit`

`pwd`

`pwd`命令用于显示当前目录，查看创建的learngit所在目录

在learngit目录下执行`git init`把这个目录变成Git可以管理的仓库

会生成一个.git目录，隐藏目录可以用`ls -ah`查看

## 克隆远程仓库

> 本地Git仓库和GitHub仓库之间的传输是通过SSH加密的
>
> Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快
>
> 使用`https`每次推送都需要输入口令

1. 创建SSH Key

   在用户主目录下执行`ssh-keygen -t rsa -C "youremail@example.com"`，然后一路回车使用默认值

   在主目录下生成一个`.ssh`目录，里面有`id_rsa`和`id_rsa_pub`,`id_rsa`是私钥，`id_rsa_pub`是公钥

2. 添加SSH Key

   在GitHub的settings   SSH and GPG keys页面添加SSH key，将`id_rsa_pub`的内容填到key处，title随意

   为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

   GitHub允许添加多个Key

3.  创建远程库

   在GitHub创建一个新的仓库，名为learngit

4.  克隆远程库

   `git clone`+远程库的SSH地址将远程库克隆到本地

   `git clone git@github.com:cxg417914077/learngit.git`

## 工作区和暂存区

工作区（Working Directory）

就是在电脑里能看到的目录，比如`learngit`文件夹就是一个工作区

版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。



![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

##  常用Git命令

`git add <file>`

将文件从工作区添加到暂存区，可以多次添加，只有添加到暂存区以后才能被下面的`commit`提交到版本库

`git commit -m <message>`

将暂存区的文件提交到仓库（提交给当前分支），`-m`后面输入的是本次提交的说明，可以输入任意内容，最好是有意义的。

`git status`

查看仓库的当前状态，指出对仓库的操作

`git diff`

`commit`提交到仓库之前查看具体修改了什么内容

`git log`

查看提交历史，显示从最近到最远的提交日志，可以在后面添加`--pretty=oneline`显示为一行输出

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`commit`后为`commit_id`

`git reflog`

用来记录你的每一次命令，查看命令历史，查看输入过的所有的命令

`git reset --hard commit_id`

回退到指定的`commit_id`对应的版本，`commit_id`可以替换成`HEAD`表示，在Git中，用`HEAD`表示当前版本上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

## 标签发布
通常的git push不会将标签对象提交到git服务器，我们需要进行显式的操作：
$ git push origin v0.1.2 # 将v0.1.2标签提交到git服务器
$ git push origin –tags # 将本地所有标签一次性提交到git服务器

注意：如果想看之前某个标签状态下的文件，可以这样操作

1.git tag   查看当前分支下的标签

2.git  checkout v0.21   此时会指向打v0.21标签时的代码状态，（但现在处于一个空的分支上）

3. cat  test.txt   查看某个文件
