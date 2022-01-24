# 日志分析ELK
一、前言

这篇文章介绍的是单独监控nginx 日志分析再进行可视化图形展示，并在用户前端使用nginx 来代理kibana的请求响应，访问权限方面暂时使用HTTP 基本认证加密用户登录。（关于elk权限控制，我所了解的还有一种方式－Shield），等以后有时间了去搞下。下面开始正文吧。。。

    注意：环境默认和上一篇大致一样，默认安装好了E、L、K、3个软件即可。当然了，还有必需的java环境JDK

开始之前，请允许我插入一张图，来自线上我的测试图:



nginx日志文件其中一行：

218.75.177.193 - - [03/Sep/2016:03:34:06 +0800] "POST /newRelease/everyoneLearnAjax HTTP/1.1" 200 370 "http://www.xxxxx.com/"
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Saf

nginx 服务器日志的log_format格式:

log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

二、配置logstash

1、修改配置文件，/etc/logstash/conf.d下。创建一个新的配置文件，内容如下:

[root@log-monitor ~]# cat /etc/logstash/conf.d/nginx_access.conf
input {
    file {
        path => [ "/data/nginx-logs/access.log" ]
        start_position => "beginning"
        ignore_older => 0
    }
}

filter {
    grok {
        match => { "message" => "%{NGINXACCESS}" }

    }
    geoip {
      source => "http_x_forwarded_for"
      target => "geoip"
      database => "/etc/logstash/GeoLiteCity.dat"
      add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
      add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}" ]
    }

    mutate {
      convert => [ "[geoip][coordinates]", "float" ]
      convert => [ "response","integer" ]
      convert => [ "bytes","integer" ]
      replace => { "type" => "nginx_access" }
      remove_field => "message"
    }

    date {
      match => [ "timestamp","dd/MMM/yyyy:HH:mm:ss Z"]

    }
    mutate {
      remove_field => "timestamp"

    }

}
output {
    elasticsearch {
        hosts => ["127.0.0.1:9200"]
        index => "logstash-nginx-access-%{+YYYY.MM.dd}"
    }
    stdout {codec => rubydebug}
}

2、文件内容大致解释
input 段：

file：使用file 作为输入源

    path: 日志的路径，支持/var/log.log，及[ “/var/log/messages”, “/var/log/.log” ] 格式

    start_position: 从文件的开始读取事件。

    另外还有end参数

    ignore_older: 忽略早于24小时（默认值86400）的日志，设为0，即关闭该功能，以防止文件中的事件由于是早期的被logstash所忽略。

    filter段：

grok:数据结构化转换工具

    match：
    匹配条件格式，将nginx日志作为message变量，并应用grok条件NGINXACCESS进行转换

geoip: 该过滤器从geoip中匹配ip字段，显示该ip的地理位置

    source：

    ip来源字段，这里我们选择的是日志文件中的最后一个字段，如果你的是默认的nginx日志，选择第一个字段即可(注：

    这里写的字段是/opt/logstash/patterns/nginx 里面定义转换后的)

    target：

    指定插入的logstash字断目标存储为geoip

    database：

    geoip数据库的存放路径

    add_field: 增加的字段，坐标经度
    add_field: 增加的字段，坐标纬度

mutate：数据的修改、删除、类型转换

    convert：将坐标转为float类型

    convert：http的响应代码字段转换成 int

    convert：http的传输字节转换成int

    replace：替换一个字段

    remove_field：移除message 的内容，因为数据已经过滤了一份，这里不必在用到该字段了。不然会相当于存两份

date: 时间处理，该插件很实用，主要是用你日志文件中事件的事件来对timestamp进行转换，导入老的数据必备！在这里曾让我困惑了很久哦。别再掉坑了

    match：匹配到timestamp字段后，修改格式为dd/MMM/yyyy:HH:mm:ss Z 　

mutate：数据修改

    remove_field：移除timestamp字段。

output段：

elasticsearch：输出到es中

    host：es的主机ip＋端口或者es 的FQDN＋端口
    index：为日志创建索引logstash-nginx-access-＊，这里也就是kibana那里添加索引时的名称

3、创建 grok 表达式
创建logstash配置文件之后，我们还要去建立grok使用的表达式，因为logstash 的配置文件里定义的使用转换格式语法，先去logstash的安装目录，默认安装位置：/opt/logstash/下，在该位置创建一个目录patterns，如下所示：

root@log-monitor ~]# mkdir -pv /opt/logstash/patterns

在该目录下创建格式文件，如下内容：

[root@log-monitor ~]# cat /opt/logstash/patterns/nginx
NGUSERNAME [a-zA-Z\.\@\-\+_%]+
NGUSER %{NGUSERNAME}
NGINXACCESS %{IPORHOST:clientip} - %{NOTSPACE:remote_user} \[%{HTTPDATE:timestamp}\] \"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) %{QS:referrer} %{QS:agent} \"%{IPV4:http_x_forwarded_for}\"

    注:该格式的最后有一个http_x_forwarded_for，因为我们日志是启用了cdn代理的。日志的第一段都是cdn的，最后一段才是真正客户的ip。

需要分析的nginx日志路径不在默认的位置，所以我根据logstash 的配置，建个目录先，并将日志文件拷贝进去：

[root@log-monitor ~]# mkdir -pv /data/nginx-logs/
[root@log-monitor ~]# ll /data/nginx-logs/
total 123476
-rw-r--r-- 1 nginx adm  126430102 Sep  9 16:02 access.log

4、配置IP库

然后就是logstash中配置的GeoIP的数据库解析ip了，这里是用了开源的ip数据源，用来分析客户端的ip归属地。官网在这里:MAXMIND

先把库下载到本地，如下操作

[root@log-monitor ~]# wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz

解压到当前路径，并将它移动到上述我们配置的路径下，当然其它路径也是可以的，不过logstash 的配置文件也需要更改，如下：

[root@log-monitor ~]# gzip -d GeoLiteCity.dat.gz
[root@log-monitor ~]# mv GeoLiteCity.dat /etc/logstash/.

测试下logstash 的配置文件，使用它自带的命令去测试，如下：

[root@log-monitor ~]# /opt/logstash/bin/logstash -t -f /etc/logstash/conf.d/nginx_access.conf
Configuration OK

    注：-t -f 参数顺序不能乱，格式就是定死的，-f 后面要跟配置文件

三、配置Elasticsearch
1、修改es的配置文件

[root@log-monitor ~]# egrep -v '^#|^$' /etc/elasticsearch/elasticsearch.yml
node.name: es-1
path.data: /data/elasticsearch/
network.host: 127.0.0.1
http.port: 9200

其它内容都保持默认。主要修改了es的数据存放路径，它默认的路径在根目录下，由于容量太小，而/data容量大。根据你的实际情况考虑而定。创建数据存放目录：

[root@log-monitor ~]# mkdir -pv /data/elasticsearch

修改该文件的权限

[root@log-monitor ~]# chown -R elasticsearch.elasticsearch /data/elasticsearch/

重启服务

[root@log-monitor ~]# systemctl restart elasticsearch
[root@log-monitor ~]# systemctl restart logstash

检查服务状态

[root@log-monitor ~]# netstat -ulntp | grep java
tcp6       0      0 127.0.0.1:9200          :::*                    LISTEN      25988/java
tcp6       0      0 127.0.0.1:9300          :::*                    LISTEN      25988/java
[root@log-monitor ~]# systemctl status logstash

查看logstash日志

[root@log-monitor ~]# tail -f /var/log/logstash/logstash.log
{:timestamp=>"2016-09-09T16:14:26.732000+0800", :message=>"Pipeline main started"}

查看es里的索引，应该已经在倒入数据了，如下

[root@log-monitor ~]# curl 'localhost:9200/_cat/indices?v'
health status index                            pri rep docs.count docs.deleted store.size pri.store.size
yellow open   .kibana                            1   1          1            0      3.1kb          3.1kb
yellow open   logstash-nginx-access-2016.09.08   5   1      69893            0     24.2mb         24.2mb
yellow open   logstash-nginx-access-2016.09.09   5   1        339            0    273.8kb        273.8kb

从上面看到数据已经在慢慢的导入了。大概需要一段时间，因为涉及到日志的过滤写入等。不过也很快啦。我们暂时不去配置kibana。先去安装nginx做个代理。
四、安装nginx 配置kibana代理
1、安装nginx

[root@log-monitor ~]# wget https://nginx.org/packages/rhel/7/x86_64/RPMS/nginx-1.10.0-1.el7.ngx.x86_64.rpm
[root@log-monitor ~]# yum localinstall nginx-1.10.0-1.el7.ngx.x86_64.rpm –y

然后新建一个elk.conf配置文件，内容如下(删除默认的配置文件：

[root@log-monitor ~]# cat /etc/nginx/conf.d/elk.conf
upstream elk {
    ip_hash;
    server 172.17.0.1:5601 max_fails=3 fail_timeout=30s;
    server 172.17.0.1:5601 max_fails=3 fail_timeout=30s;
}

server {
    listen 80;
    server_name localhost;
    server_tokens off;

    #close slow conn
    client_body_timeout 5s;
    client_header_timeout 5s;

    location / {
        proxy_pass http://elk/;
        index index.html index.htm;
        #auth
        auth_basic "ELK Private,Don't try GJ!";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }

}

2、http基本认证

[root@log-monitor ~]# yum install httpd-tools –y

新建用户：

[root@log-monitor ~]# htpasswd -cm /etc/nginx/.htpasswd elk
New password:
Re-type new password:
Adding password for user elk

启动nginx，并检查状态

[root@log-monitor ~]# systemctl start nginx
[root@log-monitor ~]# netstat -ultpn | grep :8888
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN      26424/nginx: master

3、配置防火墙

由于我们最终是使用8888端口对外提供服务的，所以kibana的5601，以及es的9200、9300端口都不需要对外

[root@log-monitor ~]# iptables -I INPUT -p tcp -m state --state NEW --dport 8888 -j ACCEPT

4、验证网站是否可正常访问

图片

输入我们建立的elk用户，登陆后，可以正常的访问kibana界面即可，如下图：

图片

添加一个索引，这个索引名字就是我们之前在logstash配置文件中导入es中的那个，本文中是logstash-nginx-access-*,如下图：

图片

查看索引，目前自由一个，设置为加星，即是discover默认突出显示的。

图片

然后我们点击Discover，即可看到我们倒入的数据了。如下图：

图片

最后这是我的dashboard，主要统计了web站点的客户端ip地址归属地、总的http传输次数、top10 来源ip、top10 请求点击页面、错误请求趋势、等等，如下，上几张图：

图片

图片

图片
五、小结

ELK 优势：

1、针对网络攻击事件时，方便运维人员查找溯源。
2、日志集中收集存储，方便后续分析
3、优化业务、系统时，做到有据可依

来源：本文转自公众号 运维部落。
