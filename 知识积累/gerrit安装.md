

wget --mirror -p --convert-links -P mirror/ https://www.gerritcodereview.com/
======================== gerrit 安装
https://www.gerritcodereview.com/releases/2.14.md

wget https://gerrit-releases.storage.googleapis.com/gerrit-2.14.3.war

adduser gerrit2

============ mysql
172.21.12.98
create database gerritdb ;

CREATE DATABASE IF NOT EXISTS gerrit248 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

/home/git/repos

============
com.google.inject.ProvisionException: Unable to provision, see the following errors:

1) No index versions ready; run java -jar /home/gerrit/bin/gerrit.war reindex

code review的目的是团队成员在一起共同学习，而不是相互”挑错”.将code review称为代码回顾好一些, 如果大家放弃”挑错”来共同学习,那么代码回顾中学习什么呢?
代码回顾的学习重点是团队成员共同识别模式，这里的模式是指程序员编写代码的习惯,包括”好模式”和”反模式”. 像富有表达力的命名, 单一职责的方法, 良好的格式缩进等,都是”好模式”. 团队成员通过阅读最近编写的测试代码和生产代码来识别”好模式”和”反模式”.既是团队成员之间相互学习的过程, 也是团队整体达成整洁代码共识的过程.

java -jar gerrit-2.11.5.war reindex

============
Configuration Error

Check the HTTP server's authentication settings.

yum -y install httpd  # 有 htpasswd 命令
htpasswd -c /home/gerrit2/passwords admin

location / {
          auth_basic              "Gerrit Code Review";
          auth_basic_user_file    /home/gerrit/passwords;
          proxy_pass              http://127.0.0.1:8081;
          proxy_set_header        X-Forwarded-For $remote_addr;
          proxy_set_header        Host $host;
        }





          auth_basic_user_file    /home/gerrit2/passwords;
          proxy_pass              http://172.21.12.73:8080;


============
我们知道，/home/gerrit/是我们之前新建的gerrit用户的，那么这个文件夹的权限是700，也就是只允许gerrit用户访问，其他组的用户是访问不了的，虽然这个文件的权限拥有root用户的所有权限，但是因为它放在700权限的文件夹下面，所以同样其他用户是访问不到的。
chmod 755 /home/gerrit

============
不显示 shh地址
需安装 plugin  download-commans
http://blog.csdn.net/cuiaamay/article/details/49994541

========================
http://blog.csdn.net/unixtech/article/details/53079041
http://www.cnblogs.com/hgj123/p/6186400.html
http://www.cnblogs.com/RainbowInTheSky/p/6142611.html

wangpanzan.com
mail.wangpanzan.com

域名解析工作。
添加一个子域名mail，A记录解析到服务器IP。
再添加一个MX记录，主机记录为空，记录值为上面解析的二级域名mail.lomu.me，优先级10。

brucemj3@163.com
[sendemail]
        enable = true
        smtpServer = smtp.163.com
        smtpServerPort = 25
        smtpUser = brucemj1@163.com
        from = brucemj1@163.com
		sslVerify = false

[sendemail]
        enable = true
        smtpServer = mail.wangpanzan.com
        smtpServerPort = 25
        smtpUser = mailtest@wangpanzan.com
        from = mailtest@wangpanzan.com
        sslVerify = false

[sendemail]
        enable = true
        smtpServer = smtp.aliyun.com
        smtpServerPort = 25
        smtpUser = youxige@aliyun.com
        from = youxige@aliyun.com
        sslVerify = false


========================
http://blog.csdn.net/leedaning/article/details/46550303
钩子的问题
[test@tomcat73 demo]$ git push origin HEAD:refs/for/master
Counting objects: 5, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 291 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done    
remote: ERROR: [3f3b593] missing Change-Id in commit message footer
remote:
remote: Hint: To automatically insert Change-Id, install the hook:
remote:   gitdir=$(git rev-parse --git-dir); scp -p -P 29418 test@172.21.12.73:hooks/commit-msg ${gitdir}/hooks/
remote: And then amend the commit:
remote:   git commit --amend
remote:
