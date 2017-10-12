# Centos7 安装 mysql5.7
[官方： yum源添加](https://dev.mysql.com/downloads/repo/yum/)
- 下载，添加源
```
wget https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
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




- ### 复制表结构
    - create table app2w as select * from appinfo where 1=2 ;

- ### 快速插入数据
    - insert  into  app2w  select * from appdemo where appinfo_id<20000 ;

- ### 设置 utf8 编码显示
    - set names utf8 ;

- ### 创建数据库
    - CREATE DATABASE IF NOT EXISTS push DEFAULT CHARSET utf8 COLLATE utf8_general_ci ;

- ### 配置用户
  - 管理员用户
```
grant all on *.* TO 'tmp'@'%' IDENTIFIED BY 'tmp';
```
  - 只读用户
```
CREATE USER 'read'@'%' IDENTIFIED BY 'read';
grant select on *.* to 'read'@'%' ;
flush privileges;
```
- ### 索引操作
    1. 添加PRIMARY KEY（主键索引）：
        - ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )
    2. 添加UNIQUE(唯一索引) ：
        - ALTER TABLE `table_name` ADD UNIQUE ( `column` )  
    3. 添加INDEX(普通索引) ：
        - ALTER TABLE `table_name` ADD INDEX index_name ( `column` )
    4. 添加FULLTEXT(全文索引) ：
        - ALTER TABLE `table_name` ADD FULLTEXT ( `column`)  
    5. 添加多列索引：
        - ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3` )
