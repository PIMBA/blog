---
title: CentOS安装.netcore应用
date: 2018-04-17 09:56:08
tags:
---

本文大部分的内容来自[msdn](https://www.microsoft.com/net/learn/get-started/linux/centos)

# 安装

订阅.net 产品

```shell
rpm --import https://packages.microsoft.com/keys/microsoft.asc
sh -c 'echo -e "[packages-microsoft-com-prod]\nname=packages-microsoft-com-prod \nbaseurl= https://packages.microsoft.com/yumrepos/microsoft-rhel7.3-prod\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/dotnetdev.repo'
```

安装.net sdk

```shell
yum update
yum install libunwind libicu
yum install dotnet-sdk-2.1.104
```

# 制作服务

在`/etc/systemd/system/`中新建一个服务文件

```shell
vim /etc/systemd/system/kestrel-ntmvc.service
```

在文件中添加

```ini
[Unit]
Description=Example .NET Web MVC Application running on Centos7

[Service]
WorkingDirectory=~/webapp/
ExecStart=/usr/bin/dotnet ~/webapp/CommVote/bin/Release/netcoreapp2.0/publish/vote.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=root
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target

```

启用服务

```shell
systemctl enable kestrel-ntmvc.service
systemctl start kestrel-ntmvc.service
systemctl status kestrel-ntmvc.service
```