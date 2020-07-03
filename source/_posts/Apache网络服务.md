---
title: Apache网络服务
date: 2020-07-02 20:00:12
---

##### 公司里有一台服务器，需要在上面跑两个网站，并且其中一个网站需要更换新域名，请问如何处理？
###### web1：www.a.com
###### web2：www.b.com------->www.d.com
<!-- more -->

#### 虚拟主机

- 基于IP
- 基于IP+端口
- 基于域名


```
cat /etc/hosts
10.12.7.12 www.a.com
10.12.7.12 www.b.com
10.12.7.12 www.d.com
```

```
NameVirtualhost *:80

<Virtualhost *:80>
  DocumentRoot /var/www/html/www.a.com
  ServerName www.b.com
</Virtualhost>

<Virtualhost *:80>
  DocumentRoot /var/www/html/www.b.com
  ServerName www.b.com
#rewrite地址重写
  <IfModule mod_rewrite.c>
  RewriteEnfine on
  RewriteCond %{HTTP_HOST} ^www.b.com
  RewriteRule ^(.*)$ http://www.d.com/$1 [R=permanent,L]
  </IfModule>
</Virtualhost>

<Virtualhost *:80>
  DocumentRoot /var/www/html/www.b.com
  ServerName www.d.com
</Virtualhost>
```



### 简述Apache的三种工作模式

###### 查看方式
###### #httpd -V | grep -i "server mpm"

###### 指定方式:
###### 在编译时,在选项中指定,--with-mpm=xxx



#### Prefork MPM

> 默认的工作模式是Prefork MPM，这种模式采用的是预派生子进程方式，用单独的子进程来处理请求，子进程间互相独立，互不影响，大大的提高了稳定性，但每个进程都会占用内存，所以消耗系统资源过高；
> 
> Prefork MPM 工作原理：控制进程Master首先会生成“StartServers”个进程，“StartServers”可以在Apache主配置文件里配置，然后为了满足“MinSpareServers”设置的最小空闲进程个数，会建立一个空闲进程，等待一秒钟，继续创建两个空闲进程，再等待一秒钟，继续创建四个空闲进程，以此类推，会不断的递归增长创建进程，最大同时创建32个空闲进程，直到满足“MinSpareServers”设置的空闲进程个数为止。Apache的预派生模式不必在请求到来的时候创建进程，这样会减小系统开销以增加性能，不过PreforkMPM是基于多进程的模式工作的，每个进程都会占用内存，这样资源消耗也较高。


#### Worker MPM

> Worker MPM是Apche 2.0版本中全新的支持多进程多线程混合模型的MPM，由于使用线程来处理HTTP请求，所以效率非常高，而对系统的开销也相对较低，Worker MPM也是基于多进程的，但是每个进程会生成多个线程，由线程来处理请求，这样可以保证多线程可以获得进程的稳定性；
> 
> Worker MPM工作原理： 控制进程Master在最初会建立“StartServers”个进程，然后每个进程会创建“ThreadPerChild”个线程，多线程共享该进程内的资源，同时每个线程独立的处理HTTP请求，为了不在请求到来的时候创建线程，WorkerMPM也可以设置最大最小空闲线程，WorkerMPM模式下同时处理的请求=ThreadPerChild*进程数，也就是MaxClients，如果服务负载较高，当前进程数不满足需求，Master控制进程会fork新的进程，最大进程数不能超过ServerLimit数，如果需要，可以调整这些对应的参数，比如，如果要调整StartServers的数量，则也要调整 ServerLimit的值


#### Event MPM

> 这个是 Apache中最新的模式，在现在版本里的已经是稳定可用的模式。它和 worker模式很像，最大的区别在于，它解决了keep-alive场景下，长期被占用的线程的资源浪费问题（某些线程因为被keep-alive，挂在那里等待，中间几乎没有请求过来，一直等到超时）。
> 
> event MPM中，会有一个专门的线程来管理这些 keep-alive 类型的线程，当有真实请求过来的时候，将请求传递给服务线程，执行完毕后，又允许它释放。这样，一个线程就能处理几个请求了，实现了异步非阻塞。
> 
> event MPM在遇到某些不兼容的模块时，会失效，将会回退到worker模式，一个工作线程处理一个请求。官方自带的模块，全部是支持event MPM的。
