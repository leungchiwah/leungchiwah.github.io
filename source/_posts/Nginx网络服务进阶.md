---
title: Nginx网络服务进阶
date: 2020-07-09 14:29:06
tags:
---
#### 写出几个Nginx的常用模块，并描述其功能

##### http_ssl_module

实现服务器加密传输的模块，部署完成后可使用https://协议进行数据传输，保证数据传输过程的安全

##### http_image_filter_module

通过该模块可以实现图片裁剪，将过大的图片裁剪为指定大小的图片，生成缩略图，保证传输速率，该选项默认不开启，需要人为指定。
<!-- more -->
```
image_filter resize $h $w;
```

##### http_rewrite_module

Nginx的地址重写模块，功能同Apache的一样，可以实现通过正则匹配来完成条件判断，然后进行域名或url重写。例如：多域名、http-->https


##### http_proxy_module

Nginx的反向代理功能，由于Nginx的高并发特性，很多时候我们都选择使用Nginx作为网站的前置服务器，一般会和upstream模块一起使用，完成压力分摊工作。

##### http_upstream_module

Nginx的负载均衡模块，一般和http_proxy模块一起使用，用来对后台服务器的任务调度及分配，分配原则可以通过算法进行控制。常见模式:Nginx+Apache、Nginx+Tomcat

---
#### 请解释Nginx是如何连接PHP进行页面解析的

Nginx支持PHP

- Nginx支持fastCGI功能（默认支持）
 
- PHP编译时开启FPM服务（编译时指定）
 
- 在Nginx配置文件中添加匹配规则（匹配后缀是.php）

#### 请描述Nginx和Tomcat之间的数据传输过程





