---
title: git操作
comments: true
date: 2017-10-30 20:59:36
tags:
- git
- git stash
- git 版本回退
---

# 使用git stash命令保存和恢复进度

## git stash

保存当前工作进度，会把暂存区和工作区的改动保存起来。执行完这个命令后，在运行`git status`命令，就会发现当前是一个干净的工作区，没有任何改动。使用`git stash save 'message...'`可以添加一些注释

## git stash list

显示保存进度的列表。也就意味着，`git stash`命令可以多次执行。

## git stash pop [–index] [stash_id]

- `git stash pop` 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
- `git stash pop --index` 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
- `git stash pop stash@{1}`恢复指定的进度到工作区。stash_id是通过`git stash list`命令得到的 
  通过`git stash pop`命令恢复进度后，会删除当前进度。

## git stash apply [–index] [stash_id]

除了不删除恢复的进度之外，其余和`git stash pop` 命令一样。

## git stash drop [stash_id]

删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

## git stash clear

删除所有存储的进度。



# git版本回退

**首先**，在本地建立一个git项目，并且与 远程服务端（github） 上的项目进行关联

1： 第一次建立git项目，提交到远程分支，并且记录为 第一个版本

 {% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-22-53.png %} 

2：更改项目中文件的内容，提交到远程分支，记录为 第二个版本

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-24-13.png %}

3：更改项目中文件的内容，提交到远程分支，记录为 第三个版本

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-24-57.png %}

本地分支的源文件的内容，如下图所示：

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-25-44.png %}

经过三次提交以后，我们可以在github上看到项目的提交记录，如下图：

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-26-36.png %} 

也可以通过在dos窗口进行查看提交历史记录, 通过 git log 命令:

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-27-13.png %}

ps： git log 命令显示从最近到最远的显示日志，我们可以看到最近三次提交；最近的是第三个版本，上一次是第二个版本，第一次是第一个版本； 如果觉得上面的 git log 显示的信息太多的话，可以使用命令 git log --pretty = online (注意是两个杠哦)

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-28-05.png %}

通过以上步骤，我们已经有三次提交记录。现在我要开始进行版本回退操作。版本回退操作，可以使用如下两种方法：

**方法1**： git reset --hard HEAD ^  ( ^ 表示回到上一个版本，如果需要回退到上上个版本的话，将HEAD^改成HEAD^^, 以此类推。那如果要回退到前100个版本，这种方法肯定不方便，我们可以使用简便命令操作：git reset --hard HEAD~100 );

未回退的之前的文件的内容为

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-29-17.png %}

现在我们将文件恢复到上一个版本的内容：

 {% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-29-53.png %}

 {% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-30-20.png %}

可以看到，文件中内容已经恢复到上一版本了，我们可以继续使用git log 来查看历史记录信息；

 {% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-30-54.png %}

我们可以看到第三个版本的信息已经看不到了，但是我如果现在又想回到第三个版本，应该怎么做呐；方法如下：

**方法2** ：git reset --hard 版本号 ，但是现在的问题是加入我已经关掉了命令行或者第三个版本的版本号，我并不知道？那么要如何知道第三个版本的版本号呐。可以通过如下命令获取到版本号： git reflog  演示如下：

 {% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-31-31.png %}

通过上面的显示我们可以知道，第三个版本的版本号是 e12928c 那么现在我们可以通过命令： git reset –hard e12928c

演示如下：

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-32-13.png %}

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-10-30_21-32-41.png %}

我们可以看到文件回到第三个版本了。

# 补充

## --mixed 

不删除工作空间改动代码，撤销commit，并且撤销git add . 操作

这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。



## --soft  

不删除工作空间改动代码，撤销commit，不撤销git add . 

 

## --hard

删除工作空间改动代码，撤销commit，撤销git add . 

注意完成这个操作后，就恢复到了上一次的commit状态。