### https://www.alihanniba.com/再启航

2016的最后一天的最后两个小时,想把新项目部署一下,先后遇到两个坑.

* 第一个是nginx 反向代理的坑,记得之前配过nodejs 的反向代理,后来服务器被荷兰的一个家伙ddos攻击导致配置全没了,也没想着备份一份.刚开始怎么配都不对,想着不应该啊,缓存设置都改了,配置信息删除,域名还是会指向nginx 默认显示.折腾半天是配置文件名起错了,nginx 配置文件应该以.conf结尾.刚好需要配置的这个忘了加了.oh my god....

* 第二个是pm2 配置问题.react 生产模式不应该走

		"transform": "react-transform-hmr"
	
	这个配置

	用

		export NODE_ENV=production && pm2 start server.js 

	启动会react-transform-hmr错误

	后来看了一下pm2文档解决了.写如下一个配置文件,以.yaml结尾

	```
	apps:
	  - script   : ./server.js
	    instances: 4
	    exec_mode: cluster
	    watch  : true
	    env    :
	      NODE_ENV: development
	    env_production:
	      NODE_ENV: production
	```

	启动如下即可:

	```
	pm2 start xxx.yaml --env production
	```
---

以下是我的nginx 反向代理配置信息.

```
server {
    listen 443;
#    listen [::]:80 default_server ipv6only=on;
    ssl on;
    ssl_certificate_key /etc/nginx/for-https/2_alihanniba.com.key;
    ssl_certificate /etc/nginx/for-https/1_alihanniba.com_bundle.crt;
    add_header Strict-Transport-Security "max-age=31536000";

#    root /var/www/alihanniba.com/public/;
#    index index.php index.html index.htm;

    gzip on;
    gzip_min_length 1k;
    gzip_comp_level 2;
    gzip_types text/plain application/javascript application-javascript text/css text/javascript application/x-httpd-php image/jpeg image/gif image/png  font/ttf font/otf image/svg+xml  ;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";

    server_name alihanniba.com www.alihanniba.com;
  location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
#        try_files $uri $uri/ /index.php?$query_string;
        # Uncomment to enable naxsi on this location
        # include /etc/nginx/naxsi.rules
#       proxy_set_header   X-Real-IP $remote_addr;
#               proxy_set_header   Host      $http_host;
        proxy_pass         http://127.0.0.1:9099;
   }

#        location ~ \.php$ {
#               try_files $uri /index.php =404;
#               fastcgi_split_path_info ^(.+\.php)(/.+)$;
#               fastcgi_pass unix:/run/php/php7.0-fpm.sock;
#               fastcgi_index index.php;
#               fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
#               include fastcgi_params;
#        }
        location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ {
 #              access_log   off;
                expires      30d;
        }

#       location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
#                access_log   off;
#                expires      24h;
#       }

        location ~* ^.+\.(html|htm)$ {
                expires      1h;
        }
        location ~* ^.+\.(eot|ttf|otf|woff|svg)$ {
                access_log   off;
                expires max;
        }
}

server {
        listen 80;
        server_name alihanniba.com www.alihanniba.com;
        rewrite ^(.*) https://$server_name$1 permanent;
}
```
目前仅仅是静态的部署,后续会加快迭代的脚步

**暂时存在两个问题**

* 服务器生产环节与本地开发环境的不同及对webpack的不熟悉导致配置不是十分准确
* webpack压缩的问题还没得到解决,导致线上请求包太大,请求速度太慢,虽说在服务器端做了gzip压缩及缓存处理,效果还是不尽人意
* 先着重把上续两问题解决了,然后再开始后台的开发.






