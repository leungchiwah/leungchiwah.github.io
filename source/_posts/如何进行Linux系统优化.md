---
title: 如何进行Linux系统优化
date: 2020-07-04 13:24:12
tags:
---
## Linux系统优化策略

#### 禁用不需要的服务
ntsysv命令最为方便

#### 避免直接使用root用户，普通用户通过sudo授权操作
<!-- more -->
#### 通过chattr锁定重要系统文件
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
/etc/inittab

#### 配置国内yum源，加快下载速度

#### 配置系统同时打开最大文件数

vi /etc/profile

ulimit -SHn 65535

#### 同步时间服务器

ntpdate ntp1.aliyun.com

通过crond定时任务，让时间同步命令每五分钟执行一次

#### 更改ssh服务的默认端口，配置SSH秘钥对登录

#### 配置合理的iptables防火墙规则

#### 配置合理的SELinux安全上下文

#### 指定合理的监控策略

#### 定时备份系统重要文件

