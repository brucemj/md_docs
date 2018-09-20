### Centos7 安装 mysql5.7
[官方： yum源添加](https://dev.mysql.com/downloads/repo/yum/)
- 下载，添加源
```
wget https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
```

- 配置版本
```
yum repolist all | grep mysql

如果 yum-config-manager 或者 dnf config-manager 可以用，可以用下面命令配置
shell> sudo yum-config-manager --disable mysql80-community
shell> sudo yum-config-manager --enable mysql57-community
或者
shell> sudo dnf config-manager --disable mysql80-community
shell> sudo dnf config-manager --enable mysql57-community


或者直接修改配置文件：
/etc/yum.repos.d/mysql-community.repo
修改想用的版本，下面变量设置为1
enabled=1
gpgcheck=1
(如果有多个enabled=1 ，会安装最新的版本 )
```

- 安装
```
yum install mysql-community-server
```
- 启动
```
service mysqld start
# 或者 /bin/systemctl start  mysqld
```
- 查看临时密码
```
less /var/log/mysqld.log
    # 密码信息： A temporary password is generated for root@localhost: Ca:hf7D8YuG;
```
- 需要强制修改临时密码
```
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
# 需要重新设置密码
SET PASSWORD = PASSWORD('123abcC!');
```
   >添加测试用户：
   >grant all on *.* TO 'tmp'@'%' IDENTIFIED BY 'tmp';
    查看mysql运行状态  systemctl status mysqld.service
    查看mysql运行状态  journalctl -xe
  - [参考 在CentOS7上安装MySQL的心路历程](http://blog.csdn.net/holmofy/article/details/69364800)



### Centos6.5 安装 mysql5.7
```
# wget https://repo.mysql.com//mysql57-community-release-el6-11.noarch.rpm
rpm -Uvh https://repo.mysql.com//mysql57-community-release-el6-11.noarch.rpm

yum install -y mysql-community-server

service mysqld start


# 如果遇到 mysql 低版本冲突的问题，可以强制卸载
rpm -qa | grep mysql
rpm -e mysql-libs-5.1.73-8.el6_8.x86_64 --nodeps

```

### ubuntu14.04 安装 mysql5.7

#### [Adding the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/#apt-repo-fresh-install)
wget https://repo.mysql.com/mysql-apt-config_0.8.4-1_all.deb
dpkg -i mysql-apt-config_0.8.4-1_all.deb
apt-get update

#### Installing MySQL with APT
[Available Packages from the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/#repo-qg-apt-available)
sudo apt-get install -y mysql-server
sudo apt-get install -y mysql-client

#### 配置
mkdir -p /konkacss/mysqlall/mysql/data/
mkdir -p /konkacss/mysqlall/logs
mv /etc/mysql/ /konkacss/mysqlall/mycnf
ln -s /konkacss/mysqlall/mycnf /etc/mysql
vim /konkacss/mysqlall/mycnf/mysql.conf.d/mysqld.cnf
```
basedir=/konkacss/mysqlall/mysql
datadir=/konkacss/mysqlall/mysql/data
log_bin=/konkacss/mysqlall/mysql/data/mysql_binary_log
```

#### 修改mysql datadir
[ubuntu修改mysql 5.7 数据存储目录datadir](http://blog.csdn.net/qq_33571718/article/details/71425623)
[ubuntu14.04 下 mysql 存储目录迁移](http://blog.csdn.net/wang794686714/article/details/39273385)

- cp -av /var/lib/mysql/* /database/mysql
- rm -rf /database/mysql/ib_logfile0
- rm -rf /database/mysql/ib_logfile1
- 在ubuntu中 有些敏感操作受到了 apparmor.d 的限制 ,mysql也受到了限制 所以要修改这个
- vim /etc/apparmor.d/usr.sbin.mysqld
```
## 目录的 / 不能少
/konkacss/mysqlall/mysql/ r,
/konkacss/mysqlall/mysql/** rwk,
```
- service apparmor reload
- service apparmor restart

- 如果遇到错误 ， 可以使用 dmesg 定位 [mysqld启动报错](https://yq.aliyun.com/articles/5841)
- 官网方法，ubuntu还需要配置 apparmor
[mysql更换datadir](https://www.digitalocean.com/community/tutorials/how-to-move-a-mysql-data-directory-to-a-new-location-on-ubuntu-16-04)
```
chown -R mysql:mysql /konkacss/mysqlall/
mysql_install_db 在5.7中不推荐使用，该功能已经整合到 mysqld

cd /konkacss/mysqlall/mysql
mysqld --initialize --user=mysql --datadir=/konkacss/mysqlall/mysql/data
mysqld --initialize-insecure --user=mysql --datadir=/konkacss/mysqlall/mysql/data
mysql_install_db --user=mysql --datadir=/konkacss/mysqlall/mysql/data
```

#### Starting and Stopping the MySQL Server
sudo service mysql status | stop | start | restart


#### 设置编码
```
a） 打开mysql配置文件：
                vim/etc/mysql/my.cnf
b） 在[client]下追加：
                default-character-set=utf8
c） 在[mysqld]下追加：
                character-set-server=utf8
d） 在[mysql]下追加：
                default-character-set=utf8
```
