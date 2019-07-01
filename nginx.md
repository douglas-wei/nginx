本文作者: **五行哥**

QQ: **1226032602**

E-mail: **1226032602@qq.com**

# web服务器种类
apache

nginx

tomcat

resin

Lighttpd

IIS

WebLogic

Jetty

Node.js
# web服务组合
比较早的，比较经典的web服务组合
LAMP（linux  apache  mysql  php）

近几年的一个web服务组合
LNMP（linux  nginx  mysql  php）
LEMP（linux  （engine   x） mysql   php）
# nginx
http://nginx.org/en/docs/
nginx本身是一款静态（html，js，css，jpg等）www软件
静态小文件高并发，同时占用资源少  3w并发   10个线程  150M

国内网站使用nginx更多一些
nginx服务从大的方面的功能
## nginx三大应用
1、 www  web 服务
2、 负载均衡 （反向代理proxy）
3、 web cache（web缓存）
[https://w3techs.com/technologies/overview/web_server/all](https://w3techs.com/technologies/overview/web_server/all)
[图片上传失败...(image-ecf6c2-1561896934467)]
## nginx功能
可针对静态资源高速高并发访问及缓存
可使用反向代理加速，并且可进行数据缓存
具有简单负载均衡、节点健康检查和容错功能
支持远程FastCGI服务的缓存加速
支持FastCGI、Uwsgi、SCGI、and Memcached Servers的加速和缓存
支持SSL、TLS、SNI
具有模块化的架构：过滤器包括gzip压缩、ranges支持、chunked响应、XSLT、SSI及图像缩放等功能。在SSI过滤器中，一个包含多个SSI的页面，如果经由FastCGI或反向代理处理，可被并行处理

支持http2.0协议
[http://nginx.org/en/docs/http/ngx_http_v2_module.html](http://nginx.org/en/docs/http/ngx_http_v2_module.html)
## nginx优点
高并发（静态小文件）， 静态 1-2W
功能种类比较多（web，cache，proxy），每个功能都不是特别强
nginx配合动态服务和apache有区别
利用nginx可以对ip限速，可以限制连接数

基于异步网络I/O模型（epoll、kqueue）使得nginx可以支持高并发
具备支持高性能，高并发的特性，并发连接可达数万 （占用资源少  2W并发  开10个线程服务，内存消耗几百M）

对小文件（小于1MB的静态文件）高并发支持好，性能很高
不支持类似apache的DSO模式，扩展库必须编译进主程序（缺点）
进程占用系统资源比较低
支持web、反向proxy、cache 三大重点功能，并且都很优秀
## nginx应用场合
1、 静态服务器（图片，视频服务），另一个lighttpd
      html，js，css，flv等
2、 动态服务，nginx+fastcgi 运行php，jsp 并发 500-1500
    apache+php， Lighttpd+fcgi php
3、反向代理，负载均衡  日pv2000W一下，都可以直接用nginx做代理
   haproxy， F5， a10
4、缓存服务 squid，varnish
## nginx作为web服务器的主要应用场合
使用nginx运行html、js、css、小图片等静态数据
nginx结合fastcgi运行php等动态程序（fastcgi_pass）
nginx 结合tomcat/resin 等支持java动态程序（proxy_pass）
## nginx和其他web服务的对比
### 静态
[图片上传失败...(image-b7c21-1561896934468)]
### 动态
[图片上传失败...(image-a09ed8-1561896934468)]
## nginx与其他web软件对比
先来看看apache的特点
高并发时消耗系统资源相对多一些
基于传统的select模型，高并发能力有限
支持扩展库，可通过DSO、apxs方法编译安装额外的插件功能，不需要重新编译apache
功能多，更稳定，更安全，插件也多
市场份额在逐年递减
[图片上传失败...(image-3b7fcd-1561896934468)]
## 如何选择web服务器
静态业务：高并发，采用nginx或lighttpd，根据自己的掌握程度或公司要求
动态业务：采用nginx和apache均可
既有静态业务又有动态：nginx或apache，不要多选，要单选
动态业务可以由前端代理（haproxy），根据页面元素的类型，向后转发相应的服务器进行处理
如果并发不是很大，又对apache很熟悉，采用apache也是可以的，apache2.4版本也很强大，
并发连接数也有所增加

nginx做web （apache、lighttpd）、反向代理（haproxy，lvs  nat）、缓存服务器 （squid）
## 反向代理或负载均衡服务
在反向代理或负载均衡服务方面，nginx可以作为web服务、php等动态服务及
memcached 缓存的代理服务器，它具有类似专业反向代理软件（如haproxy）的
功能，同时也是一个优秀的邮件代理服务软件
## 前端业务数据缓存服务
在web缓存服务方面，nginx可通过自身的proxy_cache模块实现类squid等
专业缓存软件的功能
## 为什么nginx总体性能比apache高
   nginx使用最新的epoll（linux2.6内核）和kqueue（freebsd）异步网络I/O模型，而apache则使用的是传统的select模型。目前linux下能够承受高并发访问的squid、memcached软件都采用的是epoll模型
  处理大量连接的读写，apache所采用的select网络I/O模型比较低效
# linux下软件安装
1、 rpm -ivh  *.rpm
    有依赖问题
2、 yum安装  解决rpm安装的依赖问题，安装更简单化
    优点：简单  易用  高效
    缺点：不能定制
3、源码包编译安装
./configure     #配置
make         #编译
make install    #安装
4、定制rpm包，搭建yum仓库，把我们定制的rpm包放到yum仓库，进行yum安装
    优点：2和3 的优点
    缺点：复杂
# nginx安装
## 下载nginx软件包
```
wget http://nginx.org/download/nginx-1.12.2.tar.gz
```
## 安装基础依赖包
```
yum install gcc gcc-c++ automake autoconf
```
## 安装pcre
https://regexr.com/
http://www.regexlab.com/zh/deelx/
   pcre全称（Perl Compatible Regular Expressions），中文“perl兼容正则表达式”，官方站点为
http://www.pcre.org  安装pcre库是为了使nginx支持具备URI重写功能的rewrite模块，如果不安装pcre库，则nginx无法使用rewrite模块功能，nginx的rewrite模块功能几乎是企业应用必须
```
yum install pcre pcre-devel -y
```
## 安装openssl
```
yum install openssl openssl-devel -y
```
## 安装nginx
```
tar xf nginx-1.6.2.tar.gz
cd nginx-1.6.2
useradd www -s /sbin/nologin -M
```
[http://nginx.org/en/docs/configure.html](http://nginx.org/en/docs/configure.html)
```
./configure --user=www --group=www --prefix=/application/nginx-1.6.2 --with-http_stub_status_module --with-http_ssl_module
make
make install
```
```
ln -s /application/nginx-1.6.2 /application/nginx
```
## yum安装nginx
### nginx  yum源配置
cat /etc/yum.repos.d/nginx.repo
```ini
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
```
```yml
---
- hosts: 172.16.1.7
  tasks:
  - name: Add nginx Yum Repository
    yum_repository:
      name: nginx-stable
      description: nginx-stable Repository
      baseurl: http://nginx.org/packages/centos/$releasever/$basearch/
      gpgcheck: yes
      gpgkey: https://nginx.org/keys/nginx_signing.key
```
### nginx （yum 安装）
```
yum install nginx
```
```yml
- name: install the latest version of nginx
  yum:
    name: nginx
    state: latest
  - name: copy nginx.conf
    copy:
      src: default.conf
      dest: /etc/nginx/conf.d/
    notify: restart nginx
  - name: Start service nginx, if not started
    service:
      name: nginx
      state: started
  handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```
```yml
---
- hosts: nginx
  tasks:
  - name: Add Base Yum Repository
    yum_repository:
      name: nginx-stable
      description: nginx-stable Repository
      baseurl: http://nginx.org/packages/centos/$releasever/$basearch/
      gpgcheck: yes
      gpgkey: https://nginx.org/keys/nginx_signing.key
  - name: install the latest version of nginx
    yum:
      name: nginx
      enablerepo: nginx-stable
      state: latest
  - name: copy nginx.conf
    copy:
      src: default.conf
      dest: /etc/nginx/conf.d/
    notify: restart nginx
  - name: Start service nginx, if not started
    service:
      name: nginx
      state: started
  handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```
## 启动nginx
### 检查语法
```
[root@web01 nginx]# /application/nginx/sbin/nginx -t
nginx: the configuration file /application/nginx-1.6.2/conf/nginx.conf syntax is ok
nginx: configuration file /application/nginx-1.6.2/conf/nginx.conf test is successful
```
### 启动nginx
```
[root@web01 nginx]# /application/nginx/sbin/nginx
```
## 查看端口
```
[root@web01 application]# netstat -lntup |grep nginx
tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      3770/nginx
```
## curl
```
curl -v 10.0.0.7

curl -H Host:www.etiantian.org 10.0.0.7
```
## 停止nginx
```
/application/nginx/sbin/nginx -s stop
```
## 查看nginx配置编译选项
```
[root@web01 ~]# /application/nginx/sbin/nginx -V
nginx version: nginx/1.12.2
built by gcc 4.4.7 20120313 (Red Hat 4.4.7-18) (GCC)
built with OpenSSL 1.0.1e-fips 11 Feb 2013
TLS SNI support enabled
configure arguments: --user=www --group=www --prefix=/application/nginx-1.12.2 --with-http_stub_status_module --with-http_ssl_module
```
## 测试页面
vim index.html
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to wuXing linux</h1>
<p>cheer,all,every one.</p>
<a href="https://github.com/xiadongzhi1988/wuxingge" target=_blank>wuxing github</a>.<br/>
<p><em>Thank you for wuxing_github.</em></p>
</body>
</html>
```
# nginx配置
## nginx模块
1、nginx core modules（必须的）
```
main
events
```
2、standard  http  modules  （不是必须的，默认都会安装）
```
core   核心的http参数配置，对应nginx的http区块
access  访问控制模块，用来控制网站用户对nginx的访问
fastcgi  fastcgi模块，和动态应用相关，如php
gzip    压缩模块，对nginx返回的数据压缩，性能优化模块
log     访问日志模块
proxy   proxy代理模块
rewrite   URL地址重写模块
upstream  负载均衡模块
limit_conn   限制用户并发连接数及请求数模块
limit_req   根据定义的key限制nginx请求过程的速率
auth_basic   web认证模块，设置web用户账号密码访问
ssl        ssl模块，用于加密的http连接，如https
stub_status   记录nginx基本访问状态信息
```
## nginx核心功能模块（Core functionality）
nginx核心功能模块负责nginx的全局应用，主要对应主配置文件的main区块和events区块区域，这里有很多nginx必须的全局参数配置。有关核心功能模块的详细信息官方地址
http://nginx.org/en/docs/ngx_core_module.html
| ngx_http_core_module   | 包括一些核心的http参数配置，对应nginx的配置为http区块部分   |
|----|----|
| ngx_http_access_module   | 访问控制模块，用来控制网站用户对nginx的访问   |
| ngx_http_gzip_module   | 压缩模块，对nginx返回的数据压缩，属于性能优化模块   |
| ngx_http_fastcgi_module   | fastcgi模块，和动态应用相关的模块，例如PHP   |
| ngx_http_proxy_module   | proxy代理模块   |
| ngx_http_upstream_module   | 负载均衡模块，可以实现网站的负载均衡功能及节点的健康检查   |
| ngx_http_rewrite_module   | URL地址重写模块   |
| ngx_http_limit_conn_module   | 限制用户并发连接数及请求数模块   |
| ngx_http_limit_req_module   | 根据定义的key限制nginx请求过程的速率   |
| ngx_http_log_module   | 访问日志模块，以指定的格式记录nginx客户访问日志等信息   |
| ngx_http_auth_basic_module   | web认证模块，设置web用户通过账号密码访问nginx   |
| ngx_http_ssl_module   | ssl模块，用于加密的http连接，如https   |
| ngx_http_stub_status_module   | 记录nginx基本访问状态信息等的模块   |
## **nginx主配置文件 nginx.conf (默认配置)**
```
[root@web01 conf]# egrep -v "#|^$" nginx.conf |cat -n
     1  worker_processes  1;         #worker进程的数量
     2  events {                    #事件区块开始
     3      worker_connections  1024;    #每个work进程支持的最大连接数
     4  }    #事件区块结束
     5  http {    #http区块开始
     6      include       mime.types;      #nginx支持的媒体类型库文件包含
     7      default_type  application/octet-stream;    #默认的媒体类型
     8      sendfile        on;            #开启高效传输模式
     9      keepalive_timeout  65;          #连接超时
    10      server {       #第一个server区块开始，表示一个独立的虚拟主机站点
    11          listen       80;       #提供服务的端口，默认80
    12          server_name  localhost;    #提供服务的域名主机名
    13          location / {               #第一个location区块开始
    14              root   html;          #站点的根目录，相对于nginx安装目录
    15              index  index.html index.htm;
    16          }
    17          error_page   500 502 503 504  /50x.html;
    18          location = /50x.html {
    19              root   html;
    20          }
    21      }
    22  }
```
cat nginx.conf
```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  www.etiantian.org;
        location / {
            root   html/www;
            index  oldboy.html;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```
```
mkdir ../html/www
echo oldboy >> ../html/www/oldboy.html
```
oldboy.html
```html
<!DOCTYPE html>
<html>
<meta charset="utf-8">
  <head>
    <title>
      老男孩教育xx期
    </title>
  </head>
  <body>
     welcome to oldboy linux
    <table border="9" bordercolor=#FF0000 bgcolor=#00FF00>
      <tr> <td>01</td><td>xxx</td></tr>
      <tr> <td>02</td><td>xxx</td></tr>
      <tr> <td>03</td><td>xxx</td></tr>
    </table>
    <a href="http://blog.oldboyedu.com ">
       <img src="oldboy.jpg" />
    </a>
  </body>
</html>
```
## **nginx （https配置）**
```
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
     server_name blog.wuxing.com;
       #listen       80;
       listen       443;
       ssl on;
       ssl_certificate key/server.crt;
       ssl_certificate_key key/server.key;
       location / {
          root html/blog;
            index  index.html;
        }
    }
}
```
生成证书
可以通过以下步骤生成一个简单的证书
```
cd /usr/local/nginx/conf
```
```
openssl genrsa -des3 -out server.key 1024
openssl req -new -key server.key -out server.csr
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
openssl x509 -req -days 36500 -in server.csr -signkey server.key -out server.crt
```
```
openssl genrsa -idea -out server.key 2048
openssl req -days 36500 -x509 -sha256 -nodes -newkey rsa:2048 -keyout server.key -out server.crt
```
在nginx.conf中配置这个证书
```
server {
    server_name YOUR_DOMAINNAME_HERE;
    listen 443;
    ssl on;
    ssl_certificate /usr/local/nginx/conf/server.crt;
    ssl_certificate_key /usr/local/nginx/conf/server.key;
}
```
## **nginx状态**
status.conf
```
server{
    listen  80;
    server_name  status.etiantian.org;
    location / {
      stub_status on;
      access_log   off;
      allow 10.0.0.0/24; #<==设置允许和禁止的IP段访问
      deny all;          #<==设置允许和禁止的IP段访问
    }
  }
```
[图片上传失败...(image-84101c-1561896934468)]
第一个server表示nginx启动到现在共处理了67个连接；
第二个accepts表示nginx启动到现在共成功创建67个连接；
请求丢失数=握手数-连接数   可以看出，本次状态显示没有丢失请求
第三个handled  requests 表示总共处理了202次请求；
Reading 为nginx读取到客户端的Header信息数

Writing 为nginx返回给客户端的Header信息数
Waiting 为nginx已经处理完正在等候下一次请求指令的驻留连接。在开启keep-alive的情况下，这个值等于  active-(reading+writing)
```yml
- name: get current http stats
  nginx_status_facts:
    url: http://localhost/nginx_status
    timeout: 20
```
## **虚拟主机概念**
所谓虚拟主机，在web服务里就是一个独立的网站站点，这个站点对应独立的域名（也可能是IP或端口），具有独立的程序及资源目录，可以独立地对外提供服务供用户访问。
nginx使用一个server{}标签来标示一个虚拟主机，一个web服务里可以有多个虚拟主机标签对，即同时可以支持多个虚拟主机站点
## **虚拟主机 （基于域名）**
cat nginx.conf
```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  www.etiantian.org;
            root   html/www;
            index  index.html index.htm;
    }
    server {
        listen       80;
        server_name  bbs.etiantian.org;
            root   html/bbs;
            index  index.html index.htm;
    }
    server {
        listen       80;
        server_name  blog.etiantian.org;
            root   html/blog;
            index  index.html index.htm;
    }
##status
    server {
        listen       80;
        server_name  status.etiantian.org;
          stub_status on;
          access_log   off;
    }
}
```
```
mkdir html/{www,bbs,blog} -p
echo web01 www > ../html/www/index.html
echo web01 bbs > ../html/bbs/index.html
echo web01 blog > ../html/blog/index.html
```
## **基于端口**
```
    server {
        listen       8001;
        server_name  www.etiantian.org;
            root   html/www;
            index  index.html index.htm;
            access_log  logs/www_access.log main;
    }
    server {
        listen       8002;
        server_name  bbs.etiantian.org;
            root   html/bbs;
            index  index.html index.htm;
            access_log  logs/bbs_access.log;
    }
    server {
        listen       8003;
        server_name  blog.etiantian.org;
            root   html/blog;
            index  index.html index.htm;
    }
```
## **基于ip**
添加网卡别名
```
ip addr add 10.0.0.3/24 dev eth0 label eth0:1
```
```
    server {
        listen       10.0.0.22:80;
        server_name  www.etiantian.org;
            root   html/www;
            index  index.html index.htm;
            access_log  logs/www_access.log main;
    }
    server {
        listen       10.0.0.23:80;
        server_name  bbs.etiantian.org;
            root   html/bbs;
            index  index.html index.htm;
            access_log  logs/bbs_access.log;
    }
```
## **server优先级**
多个相同的server_name 配置文件排序第一的优先访问

修改优先级
cat server3.conf
```
server {
listen 80 default_server;
server_name server3;

location / {
root /code/server3;
index index.html;
}
}
```
## **禁止ip访问（防止恶意域名绑定）**
第一个server标签
```
    server {
        listen       80;
          location / {
            deny all;
           }
    }
```
## **include**
```
http {
include mime.types;
default_type application/octet-stream;
sendfile on;
keepalive_timeout 65;

include conf.d/server1.conf;
include conf.d/server2.conf;

}
```
```
[root@web01 nginx]# ll conf.d/
total 8
-rw-r--r-- 1 root root 186 Aug 6 09:46 server1.conf
-rw-r--r-- 1 root root 182 Aug 6 09:54 server2.conf
```
## **nginx autoindex**
[http://nginx.org/en/docs/http/ngx_http_autoindex_module.html](http://nginx.org/en/docs/http/ngx_http_autoindex_module.html)
cat nginx.conf
```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   /application/yum/centos6/x86_64;
            autoindex on;
            autoindex_exact_size off;
            autoindex_localtime on;
            charset utf-8,gbk;
        }
        access_log off;
    }
}
```
# **nginx 访问控制**
[http://nginx.org/en/docs/http/ngx_http_access_module.html](http://nginx.org/en/docs/http/ngx_http_access_module.html)
## **基于IP的访问控制**
```
server {
 listen 80;
 server_name access.oldboy.com;
 root /code;
 index index.html;
 location /nginx_status {
  stub_status;
  deny 10.0.0.1;
  allow all;
 }
}
```
## **nginx认证（基于用户名和密码）**
[http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html](http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html)
```
yum install -y  httpd-tools
```
```
[root@web01 ~]# htpasswd -bc /application/nginx/conf/htpasswd oldboy 123456
Adding password for user oldboy
[root@web01 ~]# cat /application/nginx/conf/htpasswd
oldboy:FuD2.sNj9En4c

chmod 400 /application/nginx/conf/htpasswd
chown www /application/nginx/conf/htpasswd

[root@web01 conf.d]# cat basic.conf
server {
 listen 80;
 server_name basic.oldboy.com;
 root /code;
 index index.html;
 location /nginx_status {
  stub_status;
  auth_basic "Please input passwd!";
  auth_basic_user_file /application/nginx/conf/pass_file;
 }
}
```
测试
```
[root@web01 conf]# curl -u oldboy www.etiantian.org
Enter host password for user 'oldboy':
web01 www
[root@web01 conf]# curl -u oldboy:123456 www.etiantian.org
web01 www
```
# **nginx访问限制**
ngx_http_limit_conn_module
ngx_http_limit_req_module
[http://nginx.org/en/docs/http/ngx_http_limit_req_module.html](http://nginx.org/en/docs/http/ngx_http_limit_req_module.html)
cat limit_req.conf
```
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
server {
 listen 80;
 server_name req.oldboy.com;
 root /code;
 index index.html;
 location / {
  limit_req zone=one burst=3 nodelay;
 }
}
```
压测
```
ab -n 50 -c 20 http://req.oldboy.com/
```
# **nginx日志**
## **日志格式**
[http://nginx.org/en/docs/http/ngx_http_log_module.html](http://nginx.org/en/docs/http/ngx_http_log_module.html)
| 参数   | 说明   |
|----|----|
| log_format   | 用来定义记录日志的格式（可以定义多种日志格式，取不同名字即可）   |
| access_log   | 用来指定日志文件的路径及使用的何种日志格式记录日志   |
错误日志：
关键字    日志文件      日志级别
```
error_log   file          level;
```
日志级别：debug  info  notice  warn   error  crit  alert   emerg
生产环境一般是  warn  error   crit
可以放置的标签段：
main  http  server   location

访问日志
```
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```
Nginx记录日志的默认参数配置如下
```
access_log  logs/access.log  main;
```
```
$remote_addr  直接客户端地址
$http_x_forwarded_for  间接客户端地址（一般前面会有代理服务器）
$remote_user  远程客户端用户名称
$time_local  记录访问时间与时区
$request  用户的请求，使用的http协议
$status   返回状态，200，404，304等
$body_bytes_sents  发送的body字节数
$http_referer  引用页（从哪个链接访问来的）
$http_user_agent  客户端浏览器
```
```
access_log   logs/access.log   main  gzip  buffer=32k  flush=5s;
```
```
access_log  off;    #不记录访问日志
```
| $remote_addr   | 记录访问网站的客户端地址   |
|----|----|
| $remote_user   | 远程客户端用户名称   |
| $time_local   | 记录访问时间与时区   |
| $request   | 用户的http请求起始行信息   |
| $status   | http状态码，记录请求返回的状态，例如：200、404、301等   |
| $body_bytes_sent   | 服务器发送给客户端的响应body字节数   |
| $http_referer   | 记录此次请求是从哪个链接访问过来的，可以根据referer进行防盗链设置   |
| $http_user_agent   | 记录客户端访问信息，例如：浏览器，手机客户端等   |
| $http_x_forwarded_for   | 当前端有代理服务器是，设置web节点记录客户端地址的配置，此参数生效的前提是代理服务器上也要进行相关的x_forwarded_for 设置   |
## **日志存放位置**
1、全局
```
error_log  logs/error.log;
```
2、局部
```
    server {
        listen       80;
        server_name  www.etiantian.org;
            root   html/www;
            index  index.html index.htm;
            access_log  logs/www_access.log;
    }
    server {
        listen       80;
        server_name  bbs.etiantian.org;
            root   html/bbs;
            index  index.html index.htm;
            access_log  logs/bbs_access.log;
    }
```
## **日志轮询**
日志轮询脚本
cat cut_nginx_log.sh
```
#!/bin/bash
cd /application/nginx/logs && \
/bin/mv www_access.log www_access_$(date +%F -d -1day).log
/application/nginx/sbin/nginx -s reload
```
cat cut_nginx_log.sh
```
#!/bin/bash
date=`date +%F -d -1day`
cd /application/nginx/logs && \
mv www_access.log www_access_${date}.log
mv bbs_access.log bbs_access_${date}.log
mv blog_access.log blog_access_${date}.log
/application/nginx/sbin/nginx -s reload
```
cat /server/script/cut_nginx_log.sh
```
#!/bin/bash
Dateformat=`date +%Y%m%d`
Basedir="/application/nginx"
Nginxlogdir="$Basedir/logs"
Logname="access_www"
[ -d $Nginxlogdir ] && cd $Nginxlogdir||exit 1
[ -f ${Logname}.log ]||exit 1
/bin/mv ${Logname}.log ${Dateformat}_${Logname}.log
$Basedir/sbin/nginx -s reload
```
# **nginx  location**
location(位置)指令的作用是可以根据用户请求的URI来执行不同的应用，URI的知识前面已经讲过，其实就是根据用户请求的网站的地址URL匹配，匹配成功即进行相关的操作
## **location标签**
```
location[=|~|~*|^~|@] uri {
……
}
```
| location   | [ = \| ~ \| ~* \| ^~ \| @ ]   | uri   | {...}   |
|----|----|----|----|
| 指令   | 匹配标识   | 匹配的网站网址   | 匹配URI后要执行的配置段   |
### **特殊字符**
- ~   区分大小写
- ~*  不区分大小写
- !    取反
- ^~  在常规的字符串匹配检查之后，不做正则表达式的检查
### **location字符组合匹配顺序**
| 1、  location = / {"   | 精确匹配   |
|----|----|
| 2、  location ^~ /images/ {"   | 匹配常规字符串，不做正则匹配检查   |
| 3、  location ~* \.(gif\|jpg\|jpeg)$ {"   | 正则匹配   |
| 4、  location /document/ {"   | 匹配常规字符串，如果有正则则优先匹配正则   |
| 5、  location / {"   | 所有location都不能匹配后的默认匹配   |
```
curl -s -o /dev/null -I -w "%{http_code}\n"  http://www.etiantian.org
```
```
location = / {
    [ configuration A ]
}

location / {
    [ configuration B ]
}

location /documents/ {
    [ configuration C ]
}

location ^~ /images/ {
    [ configuration D ]
}

location ~* \.(gif|jpg|jpeg)$ {
    [ configuration E ]
}
```
```
        location / {
           return 401;
        }
        location = / {
            return 402;
        }

        location /documents/ {
            return 403;
        }
        location ^~ /images/ {
            return 404;
        }
         location ~* \.(gif|jpg|jpeg)$ {
            return 500;
        }
```
测试结果
```
curl -s -o /dev/null -I -w "%{http_code}\n" www.etiantian.org
402

curl -s -o /dev/null -I -w "%{http_code}\n" www.etiantian.org/index.html
401

curl -s -o /dev/null -I -w "%{http_code}\n" www.etiantian.org/documents/index.html
403

curl -s -o /dev/null -I -w "%{http_code}\n" www.etiantian.org/documents/oldgirl.jpg
500

curl -s -o /dev/null -I -w "%{http_code}\n" www.etiantian.org/images/oldboy.jpg
404
```
```
  location / {
    default_type text/html;
    return 200 "location /";
  }
```
## **location中root与alias区别**
```
[root@web01 conf.d]# cat root.conf
server {
listen 80;
server_name root.oldboy.com;
index index.html;
location /request_path/code/ {
root /local_path/code/;
}
}
```
```
mkdir /local_path/code/request_path/code/ -p
echo "Root" > /local_path/code/request_path/code/index.html
```
```
[root@web01 conf.d]# cat alias.conf
server {
listen 80;
server_name alias.oldboy.com;
index index.html;

location /request_path/code/ {
alias /local_path/code/;
}
}
```
```
echo "Alias" > /local_path/code/index.html
```
# **nginx rewrite**
## **nginx rewrite 企业应用场景**
1、 可以调整用户浏览的URL，看起来更规范，合乎开发及产品人员的需求
2、 为了让搜索引擎收录网站内容及用户体验更好，企业会将动态URL地址
伪装成静态地址提供服务
3、 网站换新域名后，让旧的域名的访问跳转到新的域名上，如：360buy  --->  jd
4、 根据特殊变量、目录、客户端的信息进行URL跳转等
## **rewrite 语法**
指令语法
```
rewrite  regex  replacement  [flag]
```
         s#regex#replacement#g
默认值：none
应用位置：server、location、if
rewrite是实现URL重写的关键指令，根据regex（正则表达式）部分内容，重定向到replacement部分内容，结尾是flag标记

在匹配过程中可以引用一些nginx的全局变量
```
$document_root  针对当前请求的根路径设置值
$host  请求信息中的"Host",如果请求中没有Host行，则等于设置的服务器名
$request_filename  当前请求的文件路径名（带网站的主目录/code/images/test.jpg）
$request_uri 当前请求的文件路径名（不带网站的主目录/images/test.jpg）
$scheme  用的协议，比如http或者https
```
flag标记说明
| last   | 本条规则匹配完成后，继续向下匹配新的location URI 规则   |
|----|----|
| break   | 本条规则匹配完成即终止，不再匹配后面的任何规则   |
| redirect   | 返回302临时重定向，浏览器地址栏会显示跳转后的URL地址   |
| permanent   | 返回301永久重定向，浏览器地址栏会显示跳转后的URL地址   |
http://oldboy.blog.51cto.com/2561410/1774260
### **对比last与break**
```
server {
    listen 80;
    server_name localhost;
    root /code;
    index index.html;
    location ~ /new.txt {
        rewrite /new.txt /old.txt break;
    }

    location ~ old.txt {
   return 333 "tttttt";
   }
}
```
```
cat /code/old.txt 
oldboy
```
cat www.conf
```
server {
        listen       80;
        server_name  www.etiantian.org etiantian.org;
        location / {
            root   html/www;
            index  index.html index.htm;
        }
        rewrite ^(.*)/bbs/  http://bbs.etiantian.org/ break;
        rewrite ^(.*)/blog/  http://blog.etiantian.org/ break;
}
```
cat www.conf
```
    server {
        listen       80;
        server_name  etiantian.org;
        access_log  logs/access_www.log  main;
        if ($host !~ "www.etiantian.org") {
        rewrite ^/(.*) http://www.etiantian.org/$1 permanent;
        }
        location / {
            root   html/www;
            index  index.html index.htm;
        }
    }
```
访问测试
```
[root@web01 conf]# curl -Lv etiantian.org
web01 www
```
### **blog.etiantian.org   跳转到   http://www.etiantian.org/blog/oldboy.html**
cat blog.conf
```
    server {
        listen       80;
        server_name  blog.etiantian.org;
        location / {
            root   html/blog;
            index  index.html index.htm;
        }
        if ( $http_host ~* "^(.*)\.etiantian\.org$") {
           set $domain $1;
           rewrite ^(.*) http://www.etiantian.org/$domain/oldboy.html break;
           }
    }
```
```
[root@web01 blog]# pwd
/application/nginx/html/www/blog
[root@web01 blog]# ll
total 4
-rw-r--r-- 1 root root 7 Oct 18 14:43 oldboy.html
```
### **http://www.etiantian.org/blog/  调转为 blog.etiantian.org**
企业blog旧的地址为：http://www.etiantian.org/blog/
现在企业新增加了新域名，希望所有用户都使用blog.etiantian.org，
但是老用户可能还会记着老地址，当这部分用户访问老地址时，
给他跳转到新地址上blog.etiantian.org
cat 01_www.conf
```
server {
  listen       80;
  server_name  www.etiantian.org etiantian.org;
  root   html/www;
  location / {
    index index.html;
    rewrite ^/blog/   http://blog.etiantian.org permanent;
  }
  access_log  logs/www_access.log main;
}
```
### **360buy.com  跳转到  jd.com**
```
server {
        listen       80;
        server_name  www.360buy.com;
        location / {
         rewrite ^/ http://www.jd.com;
        }

    }
```
### **反向代理**
```
    server {
        listen       80;
        server_name  www.360buy.com;
        location / {

             proxy_pass http://www.jd.com;
        }

     }
```
静态
```
    location  ~ ".(png|jpg|bmp|jpeg)$" {
        #rewrite  ^(.*)  http://cdn01.etiantian.org/$1  permanent ;
        return  302    $scheme://cdn01.etiantian.org/$request_uri ;

    }
```
## **wordpress伪静态**
1.在WordPress后台-设置-固定链接-自定义结构，输入下面的代码，最后保存更改即可。
/archives/%post_id%.html

2、nginx配置文件的server容器中添加下面的代码
方法1：
cat blog.conf
```
    server {
        listen       80;
        server_name  blog.xiadongzhi.com;
        root   html/blog;
        index  index.php index.html index.htm;
        location / {
        if (-f $request_filename/index.html){
            rewrite (.*) $1/index.html break;
        }
        if (-f $request_filename/index.php){
            rewrite (.*) $1/index.php;
        }
        if (!-f $request_filename){
            rewrite (.*) /index.php;
         }
        }

        location ~ .*\.(php|php5)?$ {
            root   html/blog;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }

        access_log  logs/blog_access.log main;
    }
```
方法2：
```
  location / {
    try_files $uri $uri/ /index.php?q=$uri&$args;
        }
```
# **nginx  if**
语法：
Syntax: if (condition) { ... }
Default: —
Context: server, location

运算符
```
=    !=
!~  !~*
-f  !-f
-d  !-d
-e  !-e
-x  !-x
```
```
if ($http_user_agent ~ MSIE) {
    rewrite ^(.*)$ /msie/$1 break;
}

if ($http_cookie ~* "id=([^;]+)(?:;|$)") {
    set $id $1;
}

if ($request_method = POST) {
    return 405;
}

if ($slow) {
    limit_rate 10k;
}

if ($invalid_referer) {
    return 403;
}
```
# **perl正则**
[https://www.runoob.com/perl/perl-regular-expressions.html](https://www.runoob.com/perl/perl-regular-expressions.html)
| 字符   | 描述   |
|----|----|
| \   | 将后面接着的字符标记为一个特殊字符或一个原义字符或一个后向引用。  例如，\n  匹配一个换行符，序列  \\  和  \$  匹配 $   |
| ^   | 匹配输入字符串的起始位置，如果设置regexp对象的Multiline属性， ^也匹配    \n   或  r  之后的位置   |
| $   | 匹配输入字符串的结束位置，如果设置了regexp对象的Multiline属性， $也匹配  \n  或  \r  之前的位置   |
# **lnmp**
[图片上传失败...(image-d7a4ca-1561896934468)]
[图片上传失败...(image-598de6-1561896934468)]
## **设置mysql管理员密码**
```
mysqladmin -uroot password '123456'
```
## **修改mysql管理员秘密**
```
[root@web01 ~]# mysqladmin -uroot -p password 'oldboy123'
Enter password:
Warning: Using a password on the command line interface can be insecure.
```
## **删除无用的库和用户**
```
drop database test;

mysql> select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+

mysql> select user,host from mysql.user;
+------+-----------+
| user | host      |
+------+-----------+
| root | 127.0.0.1 |
| root | ::1       |
|      | localhost |
| root | localhost |
|      | web01     |
| root | web01     |
+------+-----------+

drop user ''@'web01';
drop user 'root'@'web01';
drop user ''@'localhost';
drop user 'root'@'::1';
```
## yum 安装php7.1
```
rpm -ivh http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -ivh  https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```
```
yum -y install php71w php71w-cli php71w-common php71w-devel \
php71w-embedded php71w-gd php71w-mbstring php71w-pdo php71w-xml php71w-fpm \
php71w-mysqlnd php71w-opcache php71w-mcrypt php71w-pecl-memcached php71w-pecl-mongodb php71w-pecl-redis
```
[https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/](https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/)
[https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm](https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm)
```
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```
```
yum install php71-php php71-php-cli php71-php-common php71-php-devel \
php71-php-embedded php71-php-gd php71-php-mbstring php71-php-pdo \
php71-php-xml php71-php-fpm php71-php-mysqlnd php71-php-opcache \
php71-php-mcrypt php71-php-pecl-memcached php71-php-pecl-redis
```
```yml
---
- hosts: 172.16.1.7
  tasks:
  - name: Add php yum
    yum:
      name: https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm
      state: latest
  - name: install php
    yum:
      name:
        - php71-php
        - php71-php-cli
        - php71-php-common
        - php71-php-devel
        - php71-php-embedded
        - php71-php-gd
        - php71-php-mbstring
        - php71-php-pdo
        - php71-php-xml
        - php71-php-fpm
        - php71-php-mysqlnd
        - php71-php-opcache
        - php71-php-mcrypt
        - php71-php-pecl-memcached
        - php71-php-pecl-redis
      state: latest
  - name: copy php-fpm conf
    copy:
      src: www.conf
      dest: /etc/opt/remi/php71/php-fpm.d/www.conf
    notify: restart php-fpm
  - name: Start service php-fpm
    service:
      name: php71-php-fpm
      state: started
  handlers:
  - name: restart php-fpm
    service:
      name: php71-php-fpm
      state: restarted
```
[图片上传失败...(image-4cbdfb-1561896934468)]
Nginx不支持对外部动态程序的直接调用或者解析，所有的外部程序（包括php）必须通过FastCGI接口来调用。FastCGI接口在linux下是socket，为了调用CGI程序，还需要一个FastCGI的wrapper（可以理解为用于启动另一个程序的程序），这个wrapper绑定在某个固定的socket上，如端口或者文件socket。当nginx将CGI请求发送给这个socket的时候，通过FastCGI接口，wrapper接收到请求，然后派生出一个新的线程，这个线程调用解释器或外部程序处理脚本来读取返回的数据；接着，wrapper再将返回的数据通过FastCGI接口，沿着固定的socket传递给nginx；最后，nginx将返回的数据发送给客户端，这就是nginx+FastCGI的整个运作过程
## 源码安装php
安装依赖包
```
yum install -y zlib libxml libjpeg freetype libpng gd curl libiconv zlib-devel libxml2-devel libjpeg-devel freetype-devel libpng-devel gd-devel curl-devel libjpeg-turbo-devel libcurl-devel libxslt-devel
```
安装libiconv-1.14.tar.gz(php5.3需要)
```
tar xf libiconv-1.14.tar.gz
cd libiconv-1.14
./configure --prefix=/usr/local/libiconv
make
make install
```
```
yum -y install mhash mcrypt libmcrypt-devel
```
此处可能**lib** 或**lib64**
```
ln -s /usr/local/lib/libmcrypt.la /usr/lib64/libmcrypt.la
ln -s /usr/local/lib/libmcrypt.so /usr/lib64/libmcrypt.so
ln -s /usr/local/lib/libmcrypt.so.4 /usr/lib64/libmcrypt.so.4
ln -s /usr/local/lib/libmcrypt.so.4.4.8 /usr/lib64/libmcrypt.so.4.4.8
ln -s /usr/local/lib/libmhash.a /usr/lib64/libmhash.a
ln -s /usr/local/lib/libmhash.la /usr/lib64/libmhash.la
ln -s /usr/local/lib/libmhash.so /usr/lib64/libmhash.so
ln -s /usr/local/lib/libmhash.so.2 /usr/lib64/libmhash.so.2
ln -s /usr/local/lib/libmhash.so.2.0.1 /usr/lib64/libmhash.so.2.0.1
ln -s /usr/local/bin/libmcrypt-config /usr/bin/libmcrypt-config
```
### **php5.3**
```
./configure \
--prefix=/application/php-5.3.27 \
--with-mysql=/application/mysql \
--with-iconv-dir=/usr/local/libiconv \
--with-freetype-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib \
--with-libxml-dir=/usr \
--enable-xml \
--disable-rpath \
--enable-safe-mode \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--with-curl \
--with-curlwrappers \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--with-mcrypt \
--with-gb \
--enable-gd-native-ttf \
--with-openssl \
--with-mhash \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--enable-zip \
--enable-soap \
--enable-short-tags \
--enable-zend-multibyte \
--enable-static \
--with-xsl \
--with-fpm-user=nginx \
--with-fpm-group=nginx \
--enable-ftp
```
```
ln -s /application/mysql/lib/libmysqlclient.so.18 /usr/lib64/
touch ext/phar/phar.phar
```
### **php5.5**
```
./configure \
--prefix=/application/php-5.5.32 \
--enable-mysqlnd \
--with-mysql=mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-freetype-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib \
--with-libxml-dir=/usr \
--enable-xml \
--disable-rpath \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--with-curl \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--with-mcrypt \
--with-gd \
--with-gettext \
--enable-gd-native-ttf \
--with-openssl \
--with-mhash \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--enable-zip \
--enable-soap \
--enable-short-tags \
--enable-static \
--with-xsl \
--with-fpm-user=www \
--with-fpm-group=www \
--enable-opcache=no  \
--enable-ftp
```
```
make
make install
```
```
ln -s /application/php5.3.27 /application/php
```
### **拷贝配置文件**
```
cp php.ini-production /application/php/lib/php.ini
```
### **主配置文件**
```
/application/php/etc/php-fpm.conf
```
## **检查语法**
```
/application/php/sbin/php-fpm -t
```
## **启动php**
```
/application/php/sbin/php-fpm
```
## **查看进程**
```
netstat -lntup |grep php-fpm
tcp        0      0 127.0.0.1:9000              0.0.0.0:*                   LISTEN      50888/php-fpm
```
# **nginx整合php**
## **nginx配置文件**
cat bbs.conf
```
    server {
        listen       80;
        server_name  bbs.etiantian.org;
            root   html/bbs;
            index  index.php index.html index.htm;
            access_log  logs/bbs_access.log;
        location ~ .*\.(php|php5)?$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }

    }
```
blog.oldboy.com.conf
```
server {
        server_name blog.oldboy.com;
        listen 80;
        root /code/wordpress;
        index index.php index.html;

        location ~ \.php$ {
            root /code/wordpress;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
}
```
```
location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```
```
server {
	listen 80;
	server_name www.oldgirl.com;
	location / {
		root /html/www;
		index index.php index.html;
	}
        location ~ \.php$ {
	    root /html/www;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
}
```
## **phpinfo**
cat index.php
```php
<?php
phpinfo();
?>
```
## **php连接MySQL测试**
[https://secure.php.net/manual/zh/function.mysqli-connect.php](https://secure.php.net/manual/zh/function.mysqli-connect.php)
cat oldboy_mysql.php
```
<?php
        //$link_id=mysql_connect('主机名','用户','密码');
        $link_id=mysql_connect('localhost','root','123456') or mysql_error();
        if($link_id){
                echo "mysql successful by oldboy !\n";
        }else{
                echo "mysql_error()";

        }

?>
```
```
<?php
$link = mysqli_connect("172.16.1.7", "wordpress", "123456", "");

if (!$link) {
    echo "Error: Unable to connect to MySQL." . PHP_EOL;
    echo "Debugging errno: " . mysqli_connect_errno() . PHP_EOL;
    echo "Debugging error: " . mysqli_connect_error() . PHP_EOL;
    exit;
}

echo "Success: A proper connection to MySQL was made! The my_db database is great." . PHP_EOL;
echo "Host information: " . mysqli_get_host_info($link) . PHP_EOL;

mysqli_close($link);
?>
```
php7连接mysql测试代码
```
<?php
    $mysqli = new mysqli("172.16.1.7", "wordpress", "123456");
    if(!$mysqli)  {
        echo"database error";
    }else{
        echo"php env successful";
    }
    $mysqli->close();
?>
```
## **执行页面**
```
/application/php/bin/php oldboy_mysql.php
mysql successful by oldboy !
```
LNMP安装完成！
# **WordPress**
创建博客数据库
```
create database blog;
```
添加用户
```
grant all on blog.* to blog@'localhost' identified by 'blog';
```
# 附录
wecenter连接数据库文件
```
system/config/database.php
```
edusoho连接数据库文件
```
app/config/parameters.yml
```
**上传文件大小限制**
nginx.conf http{}中添加（如果有代理，代理也需添加）
```
client_max_body_size 300m;
```
配置 php.ini
```
upload_max_filesize = 300M
post_max_size = 300M
```
