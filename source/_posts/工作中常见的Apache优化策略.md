---
title: 工作中常见的Apache优化策略
date: 2020-07-01 14:50:41
---

###### 设置Apache的日志轮替和切割规则，防止日志文件过大
###### 美化错误页面，将错误页面重定向到首页或指定页面
###### 屏蔽Apache的版本等信息，防止别人获取Apache的相应版本
###### 配置静态缓存，减少对服务器的访问压力
###### 禁止解析指定目录下的页面程序，比如upload，禁止解析用户上传的脚本文件
---
<!-- more -->

#### 有哪些技术可以提高网站的安全和效率


- Apache服务器的安全
- Apache服务器的效率


## 日志轮替

###### 利用apache自带的rotatelogs工具进行日志切割，保证单个日志文件不要太大
 


```
CustomLog "|/bin/rotatelogs -l /wwwlogs/access_%Y%m%d.log 86400" combined
```


## 美化错误页面

###### 可以将404 500 等错误信息页面重定向到网站首页或其他页面，提升用户体验


```
vim httpd.conf

ErrorDocument 404 http://www.a.com
```


## 屏蔽apache版本等敏感信息

###### 开启子配置文件调用


```
vim httpd.conf
Include conf/extra/httpd-default.conf
```

###### 修改配置文件中默认显示的信息

```
vim httpd-default.conf
ServerTokens prod 显示 "Server: Apache"
ServerTokens Major 显示 "Server: Apache/2"
ServerTokens Minjor 显示 "Server: Apache/2.2"
ServerTokens Min 显示 "Server: Apache/2.2.17"
ServerTokens OS 显示 "Server: Apache/2.2.17 (Unix)"
ServerTokens Full 显示 "Server: Apache/2.2.17 (Unix) PHP/5.3.5"
```

## 配置静态缓存

###### 此模块默认未启用，请手动启动
###### 此模块一般写到指定的某一个网站标签中


```
vim httpd.conf
<IfModule mod_expires.c>
    ExpiresActive on
    ExpiresByType image/gif "access plus 1 days"
    ExpiresByType image/jpeg "access plus 24 hours"
    ExpiresByType image/png "access plus 1 days"
    ExpiresByType text/css "now plus 2 hours"
    ExpiresDefault "now plus 0 min"
</IfModule>
```

## 禁止解析PHP

###### 新增目录权限标签


```
vim httpd.conf

<Directory "/www/a.com/uploads">
    Options FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
    php_flag engine off
</Directory>
```
