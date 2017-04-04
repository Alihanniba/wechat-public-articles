```
#apt-get install gunicorn
#apt-get install python-dev
#apt-get install supervisor
pip install uwsgi
pip install django

```
python项目的部署之前没接触过，想着应该不会太复杂，网上copy了几份配置文件，不过还是耗费了些时间，去熟悉各个配置的意思。

目前还是用的比较人工的办法，每次部署都是手动pull push代码，然后重启uwsgi，想着之后也不会有新需求，也不会经常改动项目，也就懒得去做git hooks，毕竟没有花钱买github的私有项目

目前在服务器上也配置了虚拟环境，暂时没想到其他可以工程化的办法，依赖包都在requirements.txt这个文件里面，在服务器install一下即可，由于前端采用https，无法请求http的api，于是干脆全部https了，api不是太多，暂时只有一个展示的api，后续也不打算添加。

服务器上版本为：

* ubuntu14.04
* nginx 1.11.6
* mysql 5.7
* python 3.5.0
* pyenv
* uwsgi

由于这个服务器6月份到期，到时服务器上的所有项目都会迁移到新服务器上，域名解析也会指向新服务器IP，不打算在这个服务器上折腾了，到时候直接迁移了。

uwsgi的配置也还算简单

```
# myweb_uwsgi.ini file
[uwsgi]

# Django-related settings

socket = :8003

# the base directory (full path)
chdir           = /var/www/website/website-admin

# Django s wsgi file
module          = pysite.wsgi

# process-related settings
# master
master          = true

# maximum number of worker processes
processes       = 4

# ... with appropriate permissions - may be needed
# chmod-socket    = 664
# clear environment on exit
vacuum          = true

daemonize = /var/log/web_uwsgi.log

```
代码都在github
