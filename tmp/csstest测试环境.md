
#### csstest 测试环境账户权限
主机名  css-neixiao
域名： css.xiaobinglaile.com
IP地址： 120.77.223.234

ssh(命令部署使用):
	120.77.223.234
	csstest
	Konka123

sftp（文件上传）:
	客户端工具下载：http://172.21.5.142/tools/ftp/FileZilla_3.24.0_win64.zip
	用户名: csstest
	密码: Konka123
	目录结构：
		电子保卡: electroniccard
		用户服务: userservice
		信息采集，反馈: yws

端口分配：
	电子保卡: 8441 -- 8459
	用户服务: 8461 -- 8479
	信息采集，反馈: 8481 -- 8499

redis配置:
	安装目录: /konkacss/csstest/tools/redis
	启动命令: ./redis-server conf/redis.conf
	IP和端口: 10.11.12.186:8449

rabbitmq配置:
	rabbitmq-server: 10.11.12.186:8489
	web界面：http://css.xiaobinglaile.com:8488
	用户名，密码： admin/admin

mysql:
	外网： 120.77.223.234
	应用： 10.11.12.186
	端口： 3306
	数据库: csstest
	用户名: csstest
	密码: Konka123!@#
	SQLyog工具： http://172.21.5.142/tools/mysql/client/SQLyog10.2.zip

	建库语句:
		create database csstest default character set utf8mb4 collate utf8mb4_unicode_ci;


	my.cnf 配置：
		log_bin=/konkacss/mysql_all/mysql/mysql_binary_log
		binlog_format=ROW
		server-id=1		


	## 数据源配置
	spring.datasource.url=jdbc:mysql://10.11.12.186:3306/csstest?useUnicode=true&characterEncoding=utf8
	spring.datasource.username=csstest
	spring.datasource.password=Konka123!@#


```
  	建库语句:
  	create database csstest default character set utf8mb4 collate utf8mb4_unicode_ci;

  	CREATE USER 'csstest'@'%' IDENTIFIED BY 'Konka123!@#';

  	grant select on cssover.* to 'csstest'@'%' ;
  	grant select on weather.* to 'csstest'@'%' ;
  	grant all on csstest.* TO 'csstest'@'%' ;

  	flush privileges;
  ```

#### nginx 配置
- 172.21.12.33
	- /etc/nginx/conf.d/css.conf




#### 网络回传，手机端查询：
- 网络回传在连续带卡开机4小时后发送，若回传不成功的话 之后每半小时重试，直到成功为止

```
*#*#12125521#*#*
是指  售出时间
```










-
