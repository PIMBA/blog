---
title: CentOS安装nginx
date: 2018-04-17 10:12:42
tags:
---

# 安装

安装gcc依赖

```shell
yum -y install gcc gcc-c++ pcre pcre-devel openssl openssl-devel zlib zlib-devel
```

下载，解压安装包

```shell
wget http://nginx.org/download/nginx-1.13.5.tar.gz
mkdir nginxfiles
tar -zxvf nginx-1.13.5.tar.gz -C nginxfiles
cd nginxfiles/
cd nginx-1.13.5/
```

配置需要额外安装的模块并且编译安装

```shell
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-stream --with-mail=dynamic
make
make install
```

创建软连接

```shell
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin
```

# 配置

设置nginx自启动，在/lib/systemd/system/ 目录下创建一个服务文件

```shell
vim /lib/systemd/system/nginx.service
```

添加以下内容

```ini
[Unit]
Description=nginx - high performance web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop

[Install]
WantedBy=multi-user.target
```

启动服务

```shell
systemctl enable nginx.service
systemctl start nginx.service
systemctl status nginx.service
```

# 修改代理

编辑文件`/usr/local/nginx/conf/nginx.conf`

```shell
vim /usr/local/nginx/conf/nginx.conf
```

相应行修改为

```
server {
    listen       8000;
    #listen       somename:8080;
    #server_name  somename  alias  another.alias;

    location / {
        proxy_pass  http://localhost:50068;
        proxy_set_header Host      $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #limit_req   zone=one burst=10 nodelay;
        #limit_req_status 503;
        #root   html;
        #index  index.html index.htm;
    }
}
```

重启nginx

```shell
service nginx restart
```