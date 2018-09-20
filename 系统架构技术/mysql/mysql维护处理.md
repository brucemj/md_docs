
### 常见问题
#### 解决MySQL中Sleep连接过多的问题
```
# my.cnf
wait_timeout = 600
interactive_timeout = 600
max_connections=1000

#热部署 时间单位是秒
set global wait_timeout=600;
set global interactive_timeout=600;
set global max_connections=1000;

# 查看状态
show status
show global variables like 'max_connections';
show global status like 'max_used_connections';
show global variables like 'wait_timeout';
show global variables like 'interactive_timeout';

show full processlist;

# 如果 mysql 的sleep过多，网上有脚本关闭 Sleep
# 但是维护成本较高，不推荐使用
# 脚本参考 https://www.tuicool.com/articles/7NVRFv7
http://blog.sina.com.cn/s/blog_78ecbe330101332k.html
```
- 问题分析
```
当然，更根本的方法，还是从以上三点排查之：
1. 程序中，不使用持久链接，即使用mysql_connect而不是pconnect。
2.?? 程序执行完毕，应该显式调用mysql_close
3. 只能逐步分析系统的SQL查询，找到查询过慢的SQL,优化之p
```
- [mysql经典的8小时问题-wait_timeout](http://www.jianshu.com/p/69dcae4454b3)

### mysql启动失败
#### [实验楼挑战](https://www.shiyanlou.com/contests/lou1/challenges)
```
MySQL 服务无法启动 - bind address 配置错误
MySQL 服务无法启动 - /var/run/mysqld 权限配置错误
可以使用  sudo service mysql restart
然后查看 /var/log/mysql/error.log
在配置文件my.cnf中，把 bind_address=127.0.0.1
sudo chown mysql:msyql /var/run/mysqld

MySQL root 密码忘记 ， 把密码改为
# 使用 mysql_safe 方式启动 mysql 服务
sudo mysqld_safe --skip-grant-tables&
mysql -uroot mysql  # 免密码登录
> UPDATE user SET password=PASSWORD("shiyanlou") WHERE user='root';
> FLUSH PRIVILEGES;

```

### 记阿里云RDS故障恢复 (空间满，锁定)
#### 清除数据
[官网文档，清理数据](https://help.aliyun.com/knowledge_detail/51682.html)
```
空间满，是由于RDS实例中，数据库的数据满了，超过实例的存储空间。
和数据备份中的数据没有任何关系；因为数据备份是放在后台oss中，是专门的备份区域，不占用RDS实例的空间。

可以在 RDS的 "监控与报警" 中看到 磁盘空间 的使用情况。
具体的详细信息。可以在 RDS页面中"登录数据库"；然后查看诊断报告；
可以详细看到每个库表占用的空间情况。


[下载备份数据库](https://help.aliyun.com/knowledge_detail/41817.html)
有些命里脚本比较实用
```

#### 解锁RDS
```
数据库数据清理成功后，还是锁定状态。
在RDS控制台中没有发现解锁的功能，期间重启和RDS实例很多次，没有解锁，数据库一直是只读状态。
目前还没有更好的办法更新状态。

大概20分钟左右，实例自动回转为正常状态，数据库读写也恢复正常。
这个时间间隔比较久。
阿里工单的技术回复：
备注：锁定和解锁任务是后端间隔时间自动扫描的。
通常：10-20分钟执行自动空间扫描任务。

```

#### mysql 密码过期处理
```
# 停止mysql
/etc/init.d/mysqld stop

# 免密码方式启动mysql
mysqld_safe --skip-grant-tables &

# 登录 mysql ，并运行 sql
mysql -uroot

set password=password("Konka123_") ;
ALTER user 'root'@'%' password expire never ;
flush privileges;
```
