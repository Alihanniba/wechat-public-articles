### flask + pyenv + gunicorn + supervisor 部署python 程序踩坑
![题图](https://share.alihanniba.com/mac/2017-06-21-entrepreneur-2411763_1920.jpg)
>题图：Pixabay

#### 1. 第一次部署: **uwsgi部署**
配置文件如下：
uwsgi.ini

```
[uwsgi]
# 入口
module = run:app

master = true
processes = 5
# 线程数
threads = 100
# io
gevent = 100
async = 100

chdir  = /var/www/Bank-admin
# 端口
socket = :8005
chmod-socket = 660
vacuum = true

die-on-term = true
# 后台运行
daemonize = true
# 日志
log = /var/log/bank.log
```

启动：

```
uwsgi --ini uwsgi.ini
```

缺点： 
- 无法有效管理线程数
- 程序奔溃时无法自动重启

于是换种方式部署

===

#### 2. 第二次部署: **gunicorn部署**

```
gunicorn -w 4 -b 0.0.0.1:8005 run:app
```

缺点：
- 程序奔溃时无法自动重启

再换种方式

===

#### 3. 第三次部署: **supervisor部署**
配置文件如下：

supervisor.conf

```

[program:run]
# 程序入口
command=/root/.pyenv/versions/2.7.13/envs/bank-env-2.7.13/bin/gunicorn -w 4 -b 0.0.0.0:8005 run:app
# 程序所在目录
directory=/var/www/Bank-admin

startsecs=5

stopwaitsecs=3

autostart=true
# 自动重启
autorestart=true

stdout_logfile=/var/www/bank/log/gunicorn.log
stderr_logfile=/var/www/bank/log/gunicorn.err

```

相应命令：

```
通过配置文件启动supervisor
- supervisord -c supervisor.conf        
察看supervisor的状态                        
- supervisorctl -c supervisor.conf status  
重新载入 配置文件                  
- supervisorctl -c supervisor.conf reload  
启动指定/所有 supervisor管理的程序进程                  
- supervisorctl -c supervisor.conf start [all]|[appname]    
关闭指定/所有 supervisor管理的程序进程 
- supervisorctl -c supervisor.conf stop [all]|[appname]
```

通过命令

```
supervisord -c supervisor.conf  
```

启动后，进程跑得很正常，只是莫名其妙的会报400，所有请求都是400。google到一些结果说是需要添加allowed_hosts, 这个在程序入口处就有添加，既然**uwsgi**与**gunicorn**启动正常，应该不是这里的原因。继续排查，发现nginx转发那块配置有问题，少写了一个 **gunicorn** 配置的转发，加上即可。

添加如下，@ 符号后面的 \ 去掉，此处为显示效果需要


```
location / {
	...
	try_files $uri @\gunicorn_proxy;
	...
}
location @\gunicorn_proxy {
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header Host $http_host;
       proxy_pass http://127.0.0.1:8005;
}
```

===

截止到目前，暂时跑得正常。