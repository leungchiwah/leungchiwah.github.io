---
title: 简述Linux启动过程
date: 2020-07-04 13:22:53
tags:
---
## CentOS 6.X基本启动过程

- 服务器加点，加载BIOS信息，BIOS进行系统检测
- 加载启动引导程序（grub）
- 由grub加载系统内核
- 系统内核重新自检，并加载硬件驱动
- 由内核启动系统第一个进程/sbin/init
- 由/sbin/init进程调用/etc/init/rcS.conf，进行系统初始化配置
- 由/etc/init/rcS.conf调用/etc/inittab，确定系统的默认运行级别
- 确定默认运行级别后，调用/etc/init/rc.conf配置文件
- 运行相应的运行级别目录/etc/rc[0-6].d中的脚本
- 在启动登录界面之前，执行/etc/rc.d/rc.local中的程序
<!-- more -->

## CentOS 7.x基本启动过程

- 服务器加电。加载BIOS信息，BIOS进行系统检测
- 加载启动引导程序（grub2）
- 由grub2加载系统内核，内核重新自检
- 由grub2加载inintamfs虚拟文件系统
- 内核初始化，以加载动态模块的形式加载部分硬件的驱动
- 内核启动系统的第一个进程，也就是systemd

- systemd开始调用默认单元组（default.target），并按照默认单元组开始运行子单元组
- systemd调用sysinit.target单元组，初始化系统
- systemd调用basic.target单元组，准备操作系统
- systemd调用multi-user.target单元组，启动字符界面所需程序
- systemd调用multi-user.target单元组中的/etc/rc.d/rc.local文件，执行文件中的命令
- systemd调用multi-user.target单元组中的getty.target单元组，初始化本地终端（tty）及登录界面，如果是字符界面启动，到此启动完成
