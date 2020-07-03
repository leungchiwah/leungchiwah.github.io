---
title: 简述Linux权限划分原则
date: 2020-07-03 17:32:19
tags:
---
#### 解题思路：

- 注意权限分离（Linux系统权限、数据库权限不要掌握在同一个部门）
- 权限在满足使用的情况下，最小优先
- 减少使用root用户，尽量用“普通用户 + sudo提权” 进行日常操作
- 重要系统文件，如：/etc/passwd、/etc/shadow、/etc/fstab、/etc/sudoers等，日常建议使用chattr锁定，需要操作时再打开
- 使用脚本检测系统中新增的SUID、SGID文件
- 可以利用工具（如chkrootkit等）检测rootkit脚本
- 开启SSH服务秘钥对登录，修改SSH服务端口
