# keepalived
## lb01 keepalived
```
global_defs {
   router_id lb01
}

vrrp_script chk_nginx_proxy {
script "/server/scripts/chk_nginx_proxy.sh"
interval 1
weight -60
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id 55
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        10.0.0.3/24 dev eth0 label eth0:1
    }
    track_script {
    chk_nginx_proxy
    }
}
```
cat /server/scripts/chk_nginx_proxy.sh
```bash
#!/bin/bash
if ! ss -lntup |grep nginx &> /dev/null
then
   exit 1
else
   exit 0
fi
```
## lb02 keepalived
```
global_defs {
   router_id lb02
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id 55
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        10.0.0.3/24 dev eth0 label eth0:1
    }
}
```
# lb nginx配置
[root@lb01 conf.d]# cat proxy.conf 
```
upstream web {
	server 10.0.0.7;
	#server 10.0.0.8;
}
server {
        listen 80;
        server_name  blog.oldboy.com;
        return 302 https://$server_name$request_uri;
}
server {
	listen 443 ssl;
  ssl_certificate   /etc/nginx/server.crt;
  ssl_certificate_key  /etc/nginx/server.key;
	server_name blog.oldboy.com;
	location / {
		proxy_pass http://web;
		include proxy_params;
	}
}
```
[root@lb01 ~]# cat /etc/nginx/proxy_params 
```
proxy_redirect default;
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

proxy_connect_timeout 30;
proxy_send_timeout 60;
proxy_read_timeout 60;

proxy_buffer_size 32k;
proxy_buffering on;
proxy_buffers 4 128k;
proxy_busy_buffers_size 256k;
proxy_max_temp_file_size 256k;
```
https证书
```
cd /etc/nginx/
openssl genrsa -idea -out server.key 2048
openssl req -days 36500 -x509 -sha256 -nodes -newkey rsa:2048 -keyout server.key -out server.crt
```
# web nginx配置
[root@web01 nginx]# cat /etc/nginx/conf.d/blog.conf 
```
server {
	listen 80;
	server_name blog.oldboy.com;
	location / {
		root /html/blog;
		index index.php index.html;
	}
  location ~ \.php$ {
	    root /html/blog;
      fastcgi_pass   127.0.0.1:9000;
      fastcgi_index  index.php;
      fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
      fastcgi_param  HTTPS on;
      include        fastcgi_params;
  }
}
```
# MySQL 创建库和用户
```sql
create database wordpress;
grant all on wordpress.* to 'wordpress'@'172.16.1.%' identified by '123456';
```
测试连接MySQL
```shell
mysql -h 172.16.1.51 -uwordpress -p123456
```

