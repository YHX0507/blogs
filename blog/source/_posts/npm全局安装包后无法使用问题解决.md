---
title: npm全局安装包后无法使用问题解决
comments: true
date: 2019-04-04 20:19:36
tags:
- 不是内部命令或外部命令
- 不是可运行的程序
---

问题：`使用npm安装typescript明明安装成功，但在使用时一直报错，报错语句为  tsc不是内部或外部命令，也不是可运行的程序或批处理文件`

{% img https://yhx0507.github.io/wxapp_static/app/image_2020-07-24_20-25-53.png %}

**具体原因：node未正确安装，或相关环境变量未正确配置**

# 解决方案：

## **配置环境变量**

“我的电脑”右键“属性”-“高级系统设置”-“高级”-“环境变量”。

{% img https://yhx0507.github.io/wxapp_static/app/image_2020-07-24_20-25-53.png %}

{% img https://yhx0507.github.io/wxapp_static/app/image_2020-07-24_20-31-49.png %}

{% img https://yhx0507.github.io/wxapp_static/app/image_2020-07-24_20-32-45.png %}

 新建系统变量 NODE_PATH

进入环境变量对话框，在系统变量下新建"NODE_PATH"，输入”`D:\Program Files\Nodejs\node_global`“。

（说明：在安装nodejs时配置了全局文件夹和缓存文件夹，所以node_path 值是”D:\Program Files\Nodejs\node_global“。如果在装node时候，没有配置全局文件和缓存两个文件夹，此处NODE_PATH的值为“`用下面的没命令查看`”）

{% img https://yhx0507.github.io/wxapp_static/app/image_2020-07-24_20-38-42.png %}

`npm目录可以使用npm命令去查找：npm config get prefix`

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-07-24_20-41-28.png %}

用户变量和系统变量Path，添加值：%NODE_PATH%

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-07-24_20-42-50.png %}

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-07-24_20-45-58.png %}

在配置完变量后，关闭dos命令窗口重启dos命令，输入命令，查看是否成功。

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-07-24_20-46-39.png %}

