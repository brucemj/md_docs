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

### 复制表结果和数据，但是不包括主键，索引
create table app2w as select * from appinfo where 1=2 ;
create table app2w as select * from appinfo limit 10 ;

### 表 主键，索引 复制，数据插入
create table app2w like appdemo ;
insert  into  app2w  select * from appdemo limit 10 ;
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
