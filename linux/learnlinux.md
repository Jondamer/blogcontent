Title: Linux学习笔记
Tags: 笔记, linux
Slug: linux-notes

主要参考《循序渐进Linux》这本书

#Linux学习基本步骤

###初级阶段

1. 常用命令
2. 软件包的安装方法
3. 安装设备驱动
4. 熟悉Grub/Lilo引导程序及简单的修复操作
5. 熟悉Linux文件系统和目录结构以及Linux基本运行原理
6. 掌握常用编辑器（我选了vim），常用编辑器，调试器
7. 学习shell
8. Linux环境下的网络基本组建

###高级阶段

1. 尝试阅读Linux内核源代码（需要 c c++语言基础）
2. 尝试编译安装和调试自己的linux内核

#Linux系统内核

内核组成：

- 内存管理
- 进程管理
- 进程间通信
- 虚拟文件系统
- 网络接口

#Linux的80个主要命令

Linux默认Bourne again shell（bash）语言。

shell包含了常用的一些内置命令，用户登陆后即被载入到内存中。系统中其他的一些可执行命令也可以通过shell来解释调用。

###shell的命令格式

	command [option] [arguments]

- command：命令的名称
- options：命令的选项
- arguments：命令的参数

其中，选项是包含一个或多个字母的代码，用于定义命令的执行方式，一般前面加一个'-'用以区别参数。

###bash中的通配符

1. '*'匹配任意一个或多个字符
2. '?'匹配任意一个字符
3. '[]'匹配任何包含在方括号里面的单字符

通配符可以组合使用

###shell中的引用

bash中有一些保留字符，本身具有特殊的含义

1. 转义字符'\'

将'\'放到特殊的字符前，shell将忽略这些特殊字符的原有含义，而当成普通字符对待。

2. 单引号/双引号

放在单/双引号之间的特殊字符含义将会倍忽略

###shell的自动补全功能

tab键

##系统管理与维护命令

具体选项见书签

- ls
- pwd
- cd
- date
- passwd [用户名] 用于设置用户口令 
- man [命令名称] 用于显示指定命令的帮助信息
- who
- free -m 以MB为单位显示内存使用情况
- ps 查看系统运行状态
- top 查看进程资源占用情况

##文件管理与编辑

- mkdir 
- cat
- more
- diff
- grep 文本过滤
- rm -r删除所有子目录 -i删前提示，推荐都加上
- touch 改变指定文件的访问时间和修改时间
- ln[选项] 源文件 目标链接名   在文件或目录之间创建连接，默认hard link
- file
- cp
- find
- split
- mv

##压缩与解压

- zip [选项] 压缩文件名 需要压缩的文档列表
- unzip [选项] 压缩文件名
- gzip/gunzip
- bzip/bunzip2
- tar 作用是归档，不是压缩

##磁盘管理与维护

- df 检查系统的磁盘占用情况
- du [选项] 文件或目录  显示文件或目录所占用的磁盘空间情况
- fsck
- eject 弹出设备
- mount/umount

##网络设置与维护

- ifconfig
- netstat 显示本机网络连接，运行端口
- traceroute 同win的突然tracert

#linux下软件包的安装与管理

##源代码安装

源代码安装软件一般有以下几个步骤：

- 下载解压码源
- 分析安装平台环境（ifconfigure）
- 编译安装软件（make，make install）

###解压码源

常见的格式有'.tar.gz''tar.baz2'等，下载完成以后针对不同的软件包解压。

解压目录下一般有一个README文件，里面有相关的信息。

###分析安装环境

找到解压目录下的configure文件，可以在当前目录下输入'./configure'执行检测程序。

###编译安装软件

linux下make和makefile是常用的工具，可以简单的解决各个源文件之间复杂的依赖关系，并自动完成所有的源代码编译工作。

####makefile文件

make工具最主要的功能是通过makefile文件来实现的

##二进制格式安装

二进制文件实际上是提前编译好的，安装其实就是简单的解压缩过程

	tar -zxvf xx.tar.gz #for *.tar.gz
	tar -jxvf xx.tar.gz #for *.bz2

这类软件卸载时也只需要将目录删除掉就行了

##这里差两个

- apt-get的方式
- debian的包

##to do list

- ssh相关知识
- 系统进程管理
- 系统权限管理
- 系统文件管理
- 系统磁盘存储管理
- 系统内存管理
- 集群的基本知识
