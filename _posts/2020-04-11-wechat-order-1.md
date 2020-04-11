---
title:  "在CentOS 8下安装Nginx, MySQL, Redis"
tags: Wechat Ordering
---

# 在CentOS 8安装Nginx

参考资料：[https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-8](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-8)

## 第一步：安装

dnf 包管理系统是CentOS 8中的新默认包管理系统。

用dnf安装nginx：
```
sudo dnf install nginx
点y确认
```

开启服务，开机自动启动：
```
sudo systemctl enable nginx
sudo systemctl start nginx
```

## 第二步：防火墙

调整防火墙规则：

永久允许端口80的http连接：
```
sudo firewall-cmd --permanent --add-service=http
```

验证一下服务是否正确被添加：
```
sudo firewall-cmd --permanent --list-all
```

应该看到以下信息：
```
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: cockpit dhcpv6-client http ssh
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

为了使变更生效，需要重新加载防火墙：
```
sudo firewall-cmd --reload
```

## 第三步：测试服务器

用浏览器输入服务器的ip地址，即可访问服务器。我目前用的本地虚拟机，所以我在windows用浏览器登陆了192.168.0.136，就看到了Nginx的欢迎页面。

## 第四步：进程管理

```
sudo systemctl stop nginx # 停止服务
sudo systemctl start nginx # 开启服务
sudo systemctl restart nginx # 重启服务
sudo systemctl reload nginx # 重新加载配置
sudo systemctl disable nginx # 禁止开机自动启动
sudo systemctl enable nginx # 允许开机自动启动
```

## 第五步：重要文件和目录的位置

* 网站内容存放：`/usr/share/nginx/html`
* 服务器配置：`/etc/nginx`, `/etc/nginx/nginx.conf`, `/etc/nginx/conf.d/`
* 日志：`/var/log/nginx/access.log`, `/var/log/nginx/error.log`

# 在CentOS 8安装MySQL8.0

参考资料：

[https://www.tecmint.com/install-mysql-on-centos-8/](https://www.tecmint.com/install-mysql-on-centos-8/)

[https://www.csdn.net/gather_25/MtTaIg1sMTM3Mi1ibG9n.html](https://www.csdn.net/gather_25/MtTaIg1sMTM3Mi1ibG9n.html)

[https://computingforgeeks.com/how-to-install-mysql-8-on-fedora/](https://computingforgeeks.com/how-to-install-mysql-8-on-fedora/)

## 安装：

```
yum -y install @mysql
```

启动服务并开机自动启动：
```
systemctl start mysqld
systemctl enable --now mysqld
systemctl status mysqld
```

安全配置：
```
mysql_secure_installation
y
2
输入密码，长度至少为8，大小写都有，字母数字组合，还要有特殊字符
y
y
y
y
y
```

然后用刚刚设置的密码登陆root：
```
mysql -u root -p
输入密码：
```

## 授权用户

创建新用户：
```
在root查看所有用户：
select user, host from mysql.user;

创建一个新用户:
create user 'username'@'localhost' identified by 'password';
或者：
create user 'username'@'110.110.110.110' identified by 'password';
或者：
create user 'username'@'%' identified by 'password';
```

localhost：只有本机可以访问
110.110.110.110：用户只能从这个IP登录
%：任何电脑都可以访问

这里我保留了引号和百分号，替换了我自己设置的username，password。将来真正部署时，把百分号要换成我自己服务器的IP地址，不能让任何人都能登陆数据库。

授权：
```
GRANT ALL ON database.* TO 'username'@'localhost';
GRANT ALL ON database.* TO 'username'@'%';
```
* database：要授权的数据库
* .*：该数据库中的所有表，也可以单独指定database.table
* username：要授权的用户名
* %, localhost：要和创建时对应

刷新权限代码如下：
```
flush privileges;
```

撤销权限:
```
REVOKE ALL ON database.* FROM 'username'@'%';
REVOKE SELECT ON database.* FROM 'username'@'%';
REVOKE INSERT ON database.* FROM 'username'@'%';
REVOKE UPDATE ON database.* FROM 'username'@'%';
REVOKE DELETE ON database.* FROM 'username'@'%';
```

查看用户权限:
```
SHOW GRANTS FOR 'username'@'%';
SHOW GRANTS FOR 'username'@'localhost';
```

删除用户：
```
drop user 'username'@'localhost';
drop user 'username'@'%';
```

## 防火墙

我尝试用Navicat连接数据库，结果失败了。

确认了一下端口号：
```
mysql> show global variables like 'port';
```
确实是3306。所以不是端口号问题。

上网搜索，有的说要更改配置文件。我搜索了一下配置文件，没找到。可能是新版的CentOS的dnf安装mysql，把配置文件放到了其他位置。

然后我找到一篇英文的博客，最后一步他开放了防火墙。试了一下，成功了。

[https://computingforgeeks.com/how-to-install-mysql-8-on-fedora/](https://computingforgeeks.com/how-to-install-mysql-8-on-fedora/)

```
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --reload
```

还可以设置信任的网络，所以以后正式上线时，还应该把防火墙设置为只允许我自己的服务器访问这个数据库。
```
sudo firewall-cmd --permanent --add-rich-rule 'rule family="ipv4" \
service name="mysql" source address="10.1.1.0/24" accept'
# 第一行最后的反斜杠我觉得应该是一行写不下了，换行。这应该是一行命令。
```

# 在CentOS 8安装Redis

参考资料：
[https://linuxize.com/post/how-to-install-and-configure-redis-on-centos-8/](https://linuxize.com/post/how-to-install-and-configure-redis-on-centos-8/)

## 安装

Redis 5.0 已经被纳入了CentOS 8的默认软件库中。

```
sudo dnf install redis
# 参考资料中写的是redis-server，但我安装时没找到那个包，所以我改成了redis
```

启动服务：
```
sudo systemctl enable --now redis
```

测试：
```
sudo systemctl status redis
```

测试结果：
```
● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Sat 2020-04-11 17:12:56 EDT; 2min 44s ago
 Main PID: 4792 (redis-server)
    Tasks: 4 (limit: 23832)
   Memory: 6.6M
   CGroup: /system.slice/redis.service
           └─4792 /usr/bin/redis-server 127.0.0.1:6379

Apr 11 17:12:56 localhost.localdomain systemd[1]: Starting Redis persistent key-value database...
Apr 11 17:12:56 localhost.localdomain systemd[1]: Started Redis persistent key-value database.
```

## 远程访问配置

Redis 默认只允许本地访问。

教程说在配置文件/etc/redis.conf中，有一行是bind 127.0.0.1, 在后面添加上需要连接的IP即可。然后重启redis。

```
sudo systemctl restart redis
```

然后测试是否是监听这两个IP的6379端口：
```
ss -an | grep 6379
```

输出应该看到：
```
tcp    LISTEN    0    128    192.168.121.233:6379    0.0.0.0:*
tcp    LISTEN    0    128    127.0.0.1:6379          0.0.0.0:*
```

这一步我失败了。依然只有127.0.0.1。但是我目前还不需要远程登陆redis，所以将来需要用到时再解决吧。

