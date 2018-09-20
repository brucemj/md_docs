@import "C:\Users\mojun\.mume\style.less"
@import "E:\homewk\md_docs\css\gg\global.css"
@import "E:\homewk\md_docs\css\gg\qa.css"
@import "E:\homewk\md_docs\css\gg\responsive.css"

### 常用配置命令
#### 设置 utf8 编码显示
    set names utf8 ;

### 库 表： 创建和复制
#### 复制表结构
```
### 建库

  CREATE DATABASE IF NOT EXISTS push
    DEFAULT CHARSET utf8 COLLATE utf8_general_ci ;

    create database csstest default character set utf8mb4 collate utf8mb4_unicode_ci;


### 复制表结果和数据，但是不包括主键，索引
create table app2w as select * from appinfo where 1=2 ;
create table app2w as select * from appinfo limit 10 ;

### 表 主键，索引 复制，数据插入
create table app2w like appdemo ;
insert  into  app2w  select * from appdemo limit 10 ;
```

#### 修改表结构
```
## 重命名表
ALTER TABLE table_name RENAME TO new_table_name ;

## 增加字段
alter table tb1 add (name varchar2(30) default ‘无名氏’ not null) ;
## 增加多个字段
alter table tb1 add
 (name varchar2(30) default '无名氏' not null ,
  age integer default 22 not null
) ;

## 重命名字段
alter table tb1 rename column FIELD_NAME to NEW_FIELD_NAME ;

## 修改字段
alter table tb1 modify (name varchar2(16) default 'unknown') ;

## 删除一个字段
alter table tb1 drop column name ;

```
#### 忘记密码
```
## 停止mysql服务  service mysqld stop

## 在 my.cnf 的 [mysqld[]  中加入  skip-grant-tables
## 或者 mysqld_safe --skip-grant-tables &

## 免密码进入 sql :  mysql -uroot -p

## 修改密码
UPDATE user SET password=password('aaaaaafff') WHERE User='root';  ### 老版本
update user set authentication_string = password('aaaaaafff'), password_expired = 'N', password_last_changed = now() where user = 'root';  ### mysql5.7 新版本

set password for 'root'@'localhost'=password('aaaaaafff');  ### 该命令会报错
ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement

## 注释掉 skip-grant-tables ，正常启动 mysql

```


#### 修改数据库编码
```
alter database css CHARACTER SET utf8mb4;
```

### 索引操作
```
### 添加PRIMARY KEY（主键索引）：
ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )

### 添加UNIQUE(唯一索引) ：
ALTER TABLE `table_name` ADD UNIQUE ( `column` )  

### 添加INDEX(普通索引) ：
ALTER TABLE `table_name` ADD INDEX index_name ( `column` )

### 添加FULLTEXT(全文索引) ：
ALTER TABLE `table_name` ADD FULLTEXT ( `column`)  

### 添加多列索引：
ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3` )
```

### 配置用户
#### 管理员用户
```
grant all on *.* TO 'tmp'@'%' IDENTIFIED BY 'tmp';
```
#### 只读用户
```
CREATE USER 'read'@'%' IDENTIFIED BY 'read';
grant select on *.* to 'read'@'%' ;
flush privileges;
```

### 数据库备份
#### [学会4种备份MySQL数据库（基本备份方面没问题了）](https://www.cnblogs.com/SQL888/p/5751631.html)

### 阿里云 ECS和RDS
[使用阿里云ECS自建RDS MySQL从库](http://blog.csdn.net/dongsong1117/article/details/51800072)
[ 阿里云RDS与ECS服务器数据库做主从　[精]](http://blog.csdn.net/abcdocker/article/details/71249809)
