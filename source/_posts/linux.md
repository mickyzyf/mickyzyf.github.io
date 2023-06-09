---
layout: post
title:  "linux命令大全"
date:   2020-10-11 15:14:54
categories: Linux
tags: Linux
excerpt: 如果有ftp工具，可直接使用，但我们公司由于怕图形化界面多人传文件时容易产生覆盖和文件夹错乱的情况，修改为使用了cp的方法上传和下载
author:	朱羽飞
---


# linux命令大全

## 乱码

Linux系统中如果显示乱码：
在窗口中输入

 export LANG=zh_CN.UTF-8

## 文件的上传下载

如果有ftp工具，可直接使用，但我们公司由于怕图形化界面多人传文件时容易产生覆盖和文件夹错乱的情况，修改为使用了cp的方法上传和下载

### 上传

创建临时文件夹

 mkdir/mnt/tmp

 mount -t cifs -o username=bm,password=bm //10.0.0.11/ITDeveloper /mnt/tmp

绑定mnt/tmp文件夹，这里将bm用户上传在10.0.0.11上的ITDeveloper上的文件与linux系统中创建的临时文件绑定，此时你ls可发现tmp中的文件和ITDeveloper中的文件是一模一样的。

此时，你就可以操作文件，将tmp中的文件放在你的lunx文件系统中的任何位置

 cp -rf /复制文件路径/ /你要存放的路径/

### 下载

很简单，就是上传的逆操作，就是把文件复制的路径换一下就可以了（此方法通常用来备份文件）

## 重启服务器

 /opt/tomcat-6.0.35-xxx/bin/kdtomcat-xxx restart

上传下载文件操作完成之后，一定要重启服务器才能看见效果

## 总结

一些linux常见的命令行操作：

复制文件操作:

 cp -r dir1 dir2
把dir1中的所有文件复制到dir2文件夹下，如果dir2不存在，则可以直接使用此命令行。

如果dir2已经存在，则需要使用

 cp -r dir1/. dir2
如果仍使用 cp -r dir1 dir2则会使得dir1也复制到dir2中

 cp -rf dir1 dir2
会一个一个提示你确认覆盖操作

copy命令的功能是将给出的文件或目录拷贝到另一文件或目录中，同MSDOS下的copy命令一样，功能十分强大。

语法： cp [选项] 源文件或目录 目标文件或目录

说明：该命令把指定的源文件复制到目标文件或把多个源文件复制到目标目录中。

该命令的各选项含义如下：

1. -a 该选项通常在拷贝目录时使用。它保留链接、文件属性，并递归地拷贝目录，其作用等于dpR选项的组合。

2. -d 拷贝时保留链接。
3. -f 删除已经存在的目标文件而不提示。
4. -i 和f选项相反，在覆盖目标文件之前将给出提示要求用户确认。回答y时目标文件将被覆盖，是交互式拷贝。
5. -p 此时cp除复制源文件的内容外，还将把其修改时间和访问权限也复制到新文件中。
6. -r 若给出的源文件是一目录文件，此时cp将递归复制该目录下所有的子目录和文件。此时目标文件必须为一个目录名。
7. -l 不作拷贝，只是链接文件。

**需要说明的是，为防止用户在不经意的情况下用cp命令破坏另一个文件，如用户指定的目标文件名已存在，用cp命令拷贝文件后，这个文件就会被新源文件覆盖，因此，建议用户在使用cp命令拷贝文件时，最好使用i选项。**

## cat 命令

浏览文件的命令  -n 显示文件的时候显示行号  太长的文件不不适合用这个命令

## tac命令

逆序显示自定文件的内容(cat 倒着写)

## head 命令

正序从0行开始查看指定文件的命令 （默认显示文件最前10行）
eg:head vi.txt  查看vi.txt的内容
eg:head -n 3 vi.txt 查看vi.txt文件的前3行内容。

## tail 命令

逆序从指定行开始查看指定文件到最后一行的命令（默认显示文件最后10行）-n   -f动态显示
eg:tail -n 3 vi.txt   查看vi.txt文件最后3行的内容。

## ln命令

链接命令  link缩写   -s表示是软链接(不加该参数表示是硬链接的意思)

软链接和硬链接的区别：硬链接相当于复制，会同步更新。硬链接用的文件的-i文件号是一样的。

eg:ln abc.txt l1   创建一个硬链接l1,l1和abc.txt文件同步。

eg:ln -s abc.txt l2 创建一个软链l2,l2指向abc.txt文件，像windows里面的快捷方式。

eg:ln -s /home/local /usr  意思是将 /home/local 在/usr下创建一个快捷方式(在/usr下面也能看到local文件夹)。

## cp命令

文件复制    copy缩写

cp aaa.txt bbb.txt  将 aaa.txt文件复制(已存在)，并命名为 bbb.txt文件

cp -rf aaa bbb  将文件夹aaa复制(已存在)，并命名为 bbb文件夹    解释:-rf 这个参数是递归强制的意思

## scp命令

不同电脑之间的文件复制命令

scp mongodb-linux-x86_64-2.6.4.tgz root@119.255.27.38:/home/software/将当前系统下的 scp mongodb-linux-x86_64-2.6.4.tgz 复制到119.255.27.38的/home/software/路径下面拷贝一个文件

scp root@119.255.27.38:/home/software/jdk-7u60-linux-x64.rpm /home/software将19.255.27.38系统上的/home/software/jdk-7u60-linux-x64.rpm 文件复制到本地的/home/software路径下

scp 断点续传怎么实现？20160310

## mv命令

移动文件/文件夹     move缩写
mv   /usr/local/apache-tomcat   /usr/tomcat   将 /usr/local/apache-tomcat  移动到 /usr/ 下 且重命名为apache-tomcat

## linux下面各色文件文件夹的意思

  linux上红色背景   白色字的  还一闪一闪是什么意思啊？

  linux上红色背景 白色字 表示是错误文件或权限过高的文件或者危险文件

  eg:/usr/bin/passwd  表示权限过高的文件或者危险文件。

  linux下面红色文件的意思是表示已经断开的链接。

  文件       白色    没有执行权限

  文件       绿色    有执行权限

  文件夹   蓝色
