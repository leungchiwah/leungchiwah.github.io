---
title: Shell编程类
date: 2020-07-03 16:18:39
tags:
---
cut -d "/" -f 3
用"/"作为分隔符,截取第三字段

sort
第一次排序

uniq -c
显示该行重复的次数

sort -nr
按照数值从大到小排序
<!-- more -->
#### 面试题

###### 使用循环在/Lavacy目录下创建10个txt文件,要求文件名称由6个随机小写字母加固定字符串 (_gg) 组成,例如pzjebg_gg.txt

> 随机字符串生成
> 
> /dev/urandom 不依赖系统中断生成随机字符串,生成数据速度快但数据随机性不足(一般使用/dev/urandom)

tr命令

可以对来自标准输入的字符进行替换、压缩和删除。它可以将一组字符变成另一组字符

-c 取代所有不属于第一字符集的字符

-d 删除所有属于第一字符集的字符


```
#!/bin/bash

if [ ! -d /Lavacy ];then
        mkdir /Lavacy
fi

cd /Lavacy

for ((i=1;i<=10;i++))
  do
        filename=`tr -cd "a-z" </dev/urandom |head -c 6`
        touch ${filename}_gg.txt
done

```
> $RANDOM #此系统变量可以默认随机生成0-32767的数字
 
> echo $RANDOM

> 生成0-32767随机数

> echo $(($RANDOM%100))

> 生成100以内的随机数


---
###### 批量检查多个网站是否可以正常访问，要求使用shell数组实现，检测策略尽量模拟用户真实访问模式。

> curl 
> 
> 开源的用于数据传输的命令行工具。可以用于http访问，上传下载、用户认证、代理访问等。
> 
> 命令格式：curl 【选项】 url或者IP地址


```
#!/bin/bash
web=(
  www.baidu.com
  www.bilibili.com
  10.12.7.13
  www.zhihu.com
)
for i in ${web[*]}
do
        code=`curl -o /dev/null -s --connect-timeout 5 -w '%{http_code}' $i | grep -E "200|302"`
        if [ "$code" != "" ];then
                echo "$i is ok" >>/root/ok.log
        else
                sleep 3
                code=`curl -o /dev/null -s --connect-timeout 5 -w '%{http_code}' $i | grep -E "200|302"`
                if [ "$code" != "" ];then
                        echo "$i is ok" >>/root/ok.log
                else
                        echo "$i is error" >>/root/error.log
                fi
        fi
done

```
