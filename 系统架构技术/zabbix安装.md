[toc]

### centos6.5 安装 zabbix2.x
[CENTOS6.6下zabbix2.4.7搭建](http://www.cnblogs.com/galengao/p/5756683.html)


- find / -name "mysql_config*"
	- /usr/local/mysql/bin/mysql_config


- ./configure --prefix=/usr/local/zabbix/  --enable-server --enable-agent --with-mysql=/usr/local/mysql/bin/mysql_config --with-net-snmp --with-libcurl --with-libxml2 -enable-proxy



- yum install php-mbstring php-bcmath


- touch /var/www/html/zabbix/conf/zabbix.conf.php


- chown zabbix:zabbix /var/www/html/zabbix/conf/zabbix.conf.php


[centos6.5下Zabbix系列之Zabbix安装搭建及汉化](http://blog.chinaunix.net/xmlrpc.php?r=blog/article&uid=17238776&id=4594985)

- CentOS release 6.8 (Final) 安装 zabbix3.4
	- [root@mj34 3.4]# rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/6/x86_64/zabbix-release-3.4-1.el6.noarch.rpm
		- Retrieving http://repo.zabbix.com/zabbix/3.4/rhel/6/x86_64/zabbix-release-3.4-1.el6.noarch.rpm
		- Preparing...                ########################################### [100%] 	
		- package zabbix-release-3.4-1.el7.centos.noarch (which is newer than zabbix-release-3.4-1.el6.noarch) is already installed
		- 	file /etc/yum.repos.d/zabbix.repo from install of zabbix-release-3.4-1.el6.noarch conflicts with file from package zabbix-release-3.4-1.el7.centos.noarch

	- rpm -ivh --replacefiles --force http://repo.zabbix.com/zabbix/3.4/rhel/6/x86_64/zabbix-release-3.4-1.el6.noarch.rpm
	- 可以强制覆盖 noarch
	- yum install -y zabbix-server-mysql
	- yum install -y zabbix-web-mysql


### centos6.8 源码安装 zabbix3.4
#### zabbix3.4 服务器端
- wget http://172.21.5.142/tools/zabbix/src/zabbix-3.4.1.tar.gz
- groupadd zabbix
- useradd -g zabbix zabbix
- ./configure --prefix=/usr/local/zabbix3.4/ --enable-server --enable-agent --with-mysql=/usr/local/mysql/bin/mysql_config --enable-ipv6 --with-net-snmp --with-libcurl --with-libxml2 --with-ssh2 --with-openipmi --with-openssl --with-unixodbc
- 如果配置出错，则只产生 config.log ，如正确则有很多makefile产生
- 可能的错误信息，依赖
```
configure: error: Not found mysqlclient library      
	yum -y install mysql-devel
configure: error: LIBXML2 library not found           
	yum -y install libxml2-devel
configure: error: unixODBC library not found        
	yum -y install unixODBC-devel
configure: error: Invalid Net-SNMP directory - unable to find net-snmp-config
	yum -y install net-snmp-devel
configure: error: Invalid OPENIPMI directory - unable to find ipmiif.h
	yum -y install OpenIPMI-devel
configure: error: Curl library not found            
	yum -y install curl-devel  

### 数据库配置
create database zabbix34 character set utf8;  
grant all privileges on *.* to zabbix@'localhost' identified by 'zabbix';  
grant all privileges on *.* to zabbix@'%' identified by 'zabbix';
flush privileges;
mysql -h 172.21.12.98 -utmp -p zabbix34 < ./schema.sql
mysql -h 172.21.12.98 -utmp -p zabbix34 < ./images.sql
mysql -h 172.21.12.98 -utmp -p zabbix34 < ./data.sql


```

- web配置
	- yum -y remove $( rpm -qa | grep php )  # 移除旧版本php， 注意误删
	- yum -y install httpd php56w php56w-gd php56w-mysql php56w-bcmath php56w-mbstring php56w-xml php56w-ldap wget ntpdate net-snmp*

	- yum install -y gcc mysql-community-devel libxml2-devel  unixODBC-devel net-snmp-devel libcurl-devel libssh2-devel OpenIPMI-devel openssl-devel openldap-devel

	- chown -R apache:apache /var/www/html/zabbix


- 启动 server 和 agent
	- iptables -A INPUT -p tcp --dport 10051 -j ACCEPT
	- 配置 conf文件
	- kill $(cat /tmp/zabbix_agentd.pid )
	- kill $(cat /tmp/zabbix_server.pid )
	- /usr/local/zabbix3.4/sbin/zabbix_server
	- /usr/local/zabbix3.4/sbin/zabbix_agentd

#### zabbix3.4 客户端
#### 防火墙，端口配置
```
#vim /etc/sysconfig/iptables  
-A INPUT -m state --state NEW -m udp -p udp --dport 10050 -j ACCEPT  
-A INPUT -m state --state NEW -m tcp -p tcp --dport 10050 -j ACCEPT  
-A INPUT -m state --state NEW -m udp -p udp --dport 10051 -j ACCEPT  
-A INPUT -m state --state NEW -m tcp -p tcp --dport 10051 -j ACCEPT  
#service iptables restart  

或关闭服务器防火墙  

#service iptables stop  
```

#### ubuntu14.04 deb包安装
```
### 准备工作
## 配置ntp
18 */2 * * * /usr/sbin/ntpdate s2m.time.edu.cn
30 */2 * * * /usr/sbin/ntpdate 172.21.12.68

# 官网下载
wget http://repo.zabbix.com/zabbix/3.4/ubuntu/pool/main/z/zabbix/zabbix-agent_3.4.4-2+trusty_amd64.deb

dpkg -i zabbix-agent_3.4.4-2+trusty_amd64.deb
apt-get update

update-rc.d zabbix-agent defaults
```
```
添加angent后，发现agent和服务器通信有问题：
	Zabbix agent on ubuntu35 is unreachable for 5 minutes

查看agent日志： less /var/log/zabbix/zabbix_agentd.log
active check configuration update from [127.0.0.1:10051] started to fail (cannot connect to [[127.0.0.1]:10051]: [111] Connection refused)

配置agent:   vim /etc/zabbix/zabbix_agentd.conf
ServerActive=172.21.12.34
service zabbix-agent restart
```

#### ubuntu14.04 apt-get 安装

#### ubuntu14.04 源码 安装

#### 使用 zabbix_get
```
zabbix_get:命令行工具，手动测试数据采集
zabbix_sender:命令行工具，运行于agent端，手动向server端发送数据

在zabbix服务器可以使用下面命令测试：
/usr/local/zabbix/bin/zabbix_get -s 172.21.12.35 -p 10050 -k system.cpu.load[all,avg1]
/usr/local/zabbix/bin/zabbix_get -s 172.21.12.35 -p 10050 -k system.hostname



```
