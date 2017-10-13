# qq群技术热点

- 资产记录，没有必要通过zabbix这个来做，自己做一个cdmb，通过puppet、ansible等来抓取数据即可

- linux文件删除恢复： extundelete  [文章链接](http://www.cnblogs.com/wangxiaoqiangs/p/5630288.html)
	- 都是通过分析文件系统日志，解析出所有文件的 inode 信息，利用 inode 去查找所在 block ,利用 dd 备份出以删除的数据。
	- 当数据被删除后，立即以只读的方式重新挂载分区... 记住是立即 ！！！

- http://www.xueqing.tv  大数据课程和python
	- brucemj1@163.com----0330314Aaa


- 大部分金融公司 就是放个前置放在阿里云上, 核心还是自己机房
	- 自购服务器就是3年内自己下厨，租云主机就是3年内天天下馆子。哪样更省钱 出钱的人最清楚;产品的味道如何，食客最清楚。
	#### 我们套路是在阿里云搞得机器，做Nginx跳转。基本解决三网通了。

- [flask 源码解析：上下文](http://cizixs.com/2017/01/13/flask-insight-context)

- [Flask 的 Context 机制](https://blog.tonyseek.com/post/the-context-mechanism-of-flask/)

##### 2017年度全新上线的Python语言系列专题课，带给你不一样的学习体验！#####
- Python 网络爬虫与信息提取
	- http://www.icourse163.org/course/BIT-1001870001
- Python 数据分析与展示
	- http://www.icourse163.org/course/BIT-1001870002
- Python 机器学习应用
	- http://www.icourse163.org/course/BIT-1001872001
- Python 科学计算三维可视化
	- http://www.icourse163.org/course/BIT-1001871001
- Python 游戏开发入门
	- http://www.icourse163.org/course/BIT-1001873001
- Python 云端系统开发入门
	- http://www.icourse163.org/course/BIT-1001871002


- [Python数据分析与展示-北京理工大学](http://blog.csdn.net/linzch3/article/details/70992906)



##### Linux内核

- USTC001 LINUX操作系统分析（2017秋）
	- http://www.xuetangx.com/courses/course-v1:ustcX+USTC001+2017_T2/courseware/223ff0fcd2c94851b21c521b9cb8fb91/
- 对Linux系统的理解及学习Linux内核的心得
	- http://www.jianshu.com/p/bc6bf2c0be06

- ![周报示例](http://wangpanzan.com/image/qqgroup/weeks_demo.jpg)

- [Linux常用命令知识积累(持续更新) 简书](http://www.jianshu.com/p/efe04feca512)

- tomcat catalina.out 时间配置， timezone
	- [添加了  -Duser.timezone=GMT+08](http://sharong.iteye.com/blog/2010373)
	- 并做了内存配置的优化：
	- JAVA_OPTS="-Dfile.encoding=UTF-8 -XX:PermSize=128m -XX:MaxNewSize=256m -XX:MaxPermSize=256m -server -Xms1024m -Xmx1024m -Duser.timezone=GMT+08"


- 惊群效应
    - ngnix配置，避免 惊群效应

- cmdb

- tomcat 报错
    -  java.util.concurrent.RejectedExecutionException: Work queue full.
    - 14-Sep-2017 15:44:09.727 SEVERE [catalina-exec-555]  org.apache.coyote.http11.Http11Processor.endRequest Error finishing response
 java.lang.NullPointerException

- 在导入 sql 文件，出现Data too long for column 'brief' at row 3 错误
    -  CREATE DATABASE IF NOT EXISTS api DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
    - 或者
    - create database yourdb DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
    - set names utf8;

- ApiManager 文档管理系统
    - [ApiManager源码](https://github.com/EhsanTang/ApiManager)
    - [ApiManager文章站点](http://api.crap.cn/)
    - source /home/apimanager/apache-tomcat-8.5.20/webapps/api/CrapApi.V7.8.sql
- [开源的api文档管理系统](https://segmentfault.com/a/1190000007704665)
- [《移动开发接口及文档编写规范》V1.0](http://blog.csdn.net/tzg12345/article/details/46009421)

- [真正的inotify+rsync实时同步 彻底告别同步慢](http://blog.csdn.net/chenghuikai/article/details/50668805)


- k8s Kubernetes + Docker
- Linux 木马检查，ali云客服给的
    - chattr -i /usr/bin/.sshd; rm -f /usr/bin/.sshd;
chattr -i /usr/bin/.swhd; rm -f /usr/bin/.swhd;
rm -f -r /usr/bin/bsd-port;
cp /usr/bin/dpkgd/ps /bin/ps;
cp /usr/bin/dpkgd/netstat /bin/netstat;
cp /usr/bin/dpkgd/lsof /usr/sbin/lsof;
cp /usr/bin/dpkgd/ss /usr/sbin/ss;
rm -r -f /root/.ssh;
rm -r -f /usr/bin/bsd-port;
find /proc/ -name exe | xargs ls -l | grep -v task |grep deleted| awk '{print $11}' | awk -F/ '{print $NF}' | xargs killall -9;


- [Jekyll is a static site generation system 静态网站生成系统](https://webdesign.tutsplus.com/tutorials/how-to-set-up-a-jekyll-theme--cms-26332)

## [程序猿成长计划](https://github.com/mylxsw/growing-up)
