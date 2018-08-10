# Redis入门教程之安装redis

#redis 安装
```
ubuntu的下面安装

sudo apt install redis-servier  redis-tools
```

#phpRedisAdmin 安装
```
在安装个 phpredisAdmin的带界面管理的工具，一般我比较习惯安装个php相关的管理数据库的界面软件。
想mysql数据库有phpmyadmin， postgresql数据库有phppgadmin。

git clone https://github.com/ErikDubbelboer/phpRedisAdmin.git  /usr/share/phpRedisAdmin

把phpRedisAdmin仓库代码放到/usr/share/phpRedisAdmin下面。
-rw-rw-r-- 1 root root  206 8月  10 16:15 Dockerfile
-rw-rw-r-- 1 root root 2.7K 8月  10 16:15 README.markdown
-rw-rw-r-- 1 root root  599 8月  10 16:15 composer.json
-rw-rw-r-- 1 root root 2.3K 8月  10 16:15 composer.lock
drwxrwxr-x 2 root root 4.0K 8月  10 16:15 css/
-rw-rw-r-- 1 root root 1.8K 8月  10 16:15 delete.php
-rw-rw-r-- 1 root root  233 8月  10 16:15 docker-compose.yml
-rw-rw-r-- 1 root root 5.7K 8月  10 16:15 edit.php
-rw-rw-r-- 1 root root 4.6K 8月  10 16:15 export.php
-rw-rw-r-- 1 root root  174 8月  10 16:15 flush.php
drwxrwxr-x 2 root root 4.0K 8月  10 16:15 images/
-rw-rw-r-- 1 root root 2.3K 8月  10 16:15 import.php
drwxrwxr-x 2 root root 4.0K 8月  10 16:15 includes/
-rw-rw-r-- 1 root root 7.5K 8月  10 16:15 index.php
-rw-rw-r-- 1 root root 1.1K 8月  10 16:15 info.php
drwxrwxr-x 2 root root 4.0K 8月  10 16:15 js/
-rw-rw-r-- 1 root root 1.3K 8月  10 16:15 login.php
-rw-rw-r-- 1 root root 1.1K 8月  10 16:15 logout.php
-rw-rw-r-- 1 root root 3.4K 8月  10 16:15 overview.php
-rw-rw-r-- 1 root root 1.2K 8月  10 16:15 rename.php
-rw-rw-r-- 1 root root  325 8月  10 16:15 save.php
-rw-rw-r-- 1 root root 1.1K 8月  10 16:15 ttl.php
drwxrwxr-x 7 mamh mamh 4.0K 8月  10 16:28 vendor/
-rw-rw-r-- 1 root root  13K 8月  10 16:15 view.php


然后在下载个依赖
git clone https://github.com/nrk/predis.git /usr/share/phpRedisAdmin/vendor

要注意代码放到路径，直接下载到/usr/share 可能是没权限的。特别注意一些就行。


最后配置apache
在apache配置目录下面新建个conf-available/phpredisadmin.conf配置文件。内容如下
$ cat conf-available/phpredisadmin.conf    [mamh@10.0.63.21 ] 18-08-10 16:57  /etc/apache2

Alias /phpredisadmin /usr/share/phpRedisAdmin
Alias /phpRedisAdmin /usr/share/phpRedisAdmin

<Directory /usr/share/phpRedisAdmin>

<IfModule mod_dir.c>
DirectoryIndex index.php
</IfModule>
AllowOverride None

# Only allow connections from localhost:
#Require local
allow from all

<IfModule mod_php.c>
  php_flag magic_quotes_gpc Off
  php_flag track_vars On
  #php_value include_path .
</IfModule>
<IfModule !mod_php.c>
  <IfModule mod_actions.c>
    <IfModule mod_cgi.c>
      AddType application/x-httpd-php .php
      Action application/x-httpd-php /cgi-bin/php
    </IfModule>
    <IfModule mod_cgid.c>
      AddType application/x-httpd-php .php
      Action application/x-httpd-php /cgi-bin/php
    </IfModule>
  </IfModule>
</IfModule>

</Directory>

然后是 apache配置目录下面的conf-enabled目录下面建立个软连接：
phpredisadmin.conf -> ../conf-available/phpredisadmin.conf

最后重启apache 服务器：sudo service apache2 restart

打开浏览器访问  http://localhost/phpRedisAdmin   即可。
```

