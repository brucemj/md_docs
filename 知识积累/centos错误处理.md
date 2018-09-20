#### yum epel的扩展报错
```
今天给Centos通过rpm -Uvh装了个epel的扩展后，执行yum就开始报错：
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again

在网上查了查，解决办法都是编辑/etc/yum.repos.d/epel.repo，把基础的恢复，镜像的地址注释掉
#baseurl
mirrorlist
改成
baseurl
#mirrorlist
```
 - [ Cannot retrieve metalink for repository: epel…错误解决办法](http://blog.csdn.net/shile/article/details/52250566)
 ```
RHEL6下yum -y install epel-release安装了epel源,但yum makecache出错。
centos下安装完EPEL源然后更新一下yum缓存如果发现这样的错误:
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
这就表明你需要更新CA证书了，那么只需要更新CA证书就可以，不过在此同时需要临时禁用epel源并更新就可以了，命令如下：
yum --disablerepo=epel -y update ca-certificates
 ```

#### yum  update 和 upgrade 区别
```
yum --help 解释
update         Update a package or packages on your system
update-minimal Works like update, but goes to the 'newest' package match which fixes a problem that affects your system
updateinfo     Acts on repository update information
upgrade        Update packages taking obsoletes into account


yum -y update
升级所有包，改变软件设置和系统设置,系统版本内核都升级
yum -y upgrade
升级所有包，不改变软件设置和系统设置，系统版本升级，内核不改变
```
