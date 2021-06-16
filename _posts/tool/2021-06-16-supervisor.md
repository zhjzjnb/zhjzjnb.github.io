---
layout: post
title: supervisor 使用
category: 工具
tags: supervisor
keywords: supervisor
---

## supervisor是用python研发的守护进程

#### 基本操作

- 安装

```
apt-get install supervisor
```

- 配置文件 supervisord.conf

```
[unix_http_server]
file=/var/run/supervisor.sock   ; (the path to the socket file)
chmod=0700                       ; sockef file mode (default 0700)

[inet_http_server]         ; web管理页面 侦听在TCP上的socket，Web Server和远程的supervisorctl都要用到他不设置的话，默认为不开启。非必须设置
port=*:9001                ; 这个是侦听的IP和端口，侦听所有IP用 :9001或*:9001 127.0.0.1:9001 为本地
username=user              ; 账号
password=123               ; 密码
 
[supervisord]
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)


[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket


[include]
files = /etc/supervisor/conf.d/*.conf
```

- app 配置文件

```
[program:app-web]
directory=/root/spring-boot ; 程序的启动目录
command=java -jar app-web.jar ; 启动命令
autostart=false     ; 在 supervisord 启动的时候也自动启动
startsecs=30        ; 启动 30 秒后没有异常退出，就当作已经正常启动了
autorestart=true   ; 程序异常退出后自动重启
startretries=3     ; 启动失败自动重试次数，默认是 3
user=root          ; 用哪个用户启动
redirect_stderr=true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes=100MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups=2     ; stdout 日志文件备份数  ; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录
stdout_logfile=/root/spring-boot/app-web.out ;应用日志目录
```

- 更新配置文件

```
supervisorctl update
```

- 停止服务

```
supervisorctl shutdown
```


- 操作app

```
supervisorctl status  查看状态

supervisorctl start app=([program:app-web]配置后缀)

supervisorctl stop app=([program:app-web]配置后缀)
```

### 问题记录

supervisorctl 需要到supervisord.conf目录执行,否则需要指定-c
