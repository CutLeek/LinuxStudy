# Nginx配置

## 编译安装nginx

### 安装依赖库

```shell
yum -y install gcc gcc-c++ automake pcre pcre-devel zlib zlib-devel openssl openssl-devel 
```

### 下载nginx源码包

```shell
wget http://nginx.org/download/nginx-1.8.1.tar.gz
```

### 解压到指定目录

```shell
tar xvf nginx-1.8.1.tar.gz && cd nginx-1.8.1
```

### 预编译

```shell
./configure  --prefix=/data/nginx  --sbin-path=/data/nginx/sbin/nginx --conf-path=/data/nginx/conf/nginx.conf --error-log-path=/var/log/nginx/error.log  --http-log-path=/var/log/nginx/access.log  --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock  --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module --http-client-body-temp-path=/var/tmp/nginx/client/ --http-proxy-temp-path=/var/tmp/nginx/proxy/ --http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ --http-uwsgi-temp-path=/var/tmp/nginx/uwsgi --http-scgi-temp-path=/var/tmp/nginx/scgi --with-pcre
```

### 编译安装

```shell
make && make install
```

## 编译安装php

### 安装 依赖库

```shell
yum install -y gcc gcc-c++  make zlib zlib-devel pcre pcre-devel  libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers sqlite-devel
```

### 下载php安装包

```shell
wget http://mirrors.sohu.com/php/php-7.4.3.tar.gz
```

### 解压到指定目录

```
tar xvf php-7.4.3.tar.gz && cd php-7.4.3
```

### 预编译

```
./configure --prefix=/data/php7 --enable-fpm
```

### 编译安装

````
make && make install
````

### 配置php

1、为php命令建立软链接，加入到环境变量中

```
ln -s /data/php7/bin/php /usr/local/bin/php
```

2、创建配置文件，并将其复制到正确的位置

```
cp php.ini-development /data/php7/lib/php.ini
```

3、为php-fpm命令建立软链接，加入到环境变量中

```
ln -s /data/php7/sbin/php-fpm /usr/local/sbin/php-fpm
```

4、复制php配置文件目录下的 php-fpm.conf.default，并重命名为 php-fpm.conf

```
cp /data/php7/etc/php-fpm.conf.default /data/php7/etc/php-fpm.conf
```

5、复制php配置文件目录下的 php-fpm.d/www.conf.default，并重命名为 php-fpm.d/www.conf

```
cp /data/php7/etc/php-fpm.d/www.conf.default /data/php7/etc/php-fpm.d/www.conf
```

6、编辑 php-fpm.d/www.conf，设置 php-fpm 模块使用 www-data 用户和 www-data 用户组的身份运行

```shell
vim /data/php7/etc/php-fpm.d/www.conf

user = www-data
group = www-data
```

7、编辑 php.ini，文件中的配置项 cgi.fix_pathinfo 设置为 0 。

````
vim /data/php7/lib/php.ini

cgi.fix_pathinfo=0
````

8、启动php-fpm

```
php-fpm
```

### 配置nginx使其支持php

```nginx
location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

## nginx配置文件详解





















