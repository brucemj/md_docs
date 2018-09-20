
### lost+found 目录的作用
```
如果你运行fsck命令（文件系统检查和修复命令），它也许会找到一些数据碎片，这些文件碎片在硬盘中并没有引用。特别的，fsck也许能找到看起来是完整的文件，但是在系统中没有名字－一个inode但是不对应文件名。这个数据仍然占用硬盘空间，但是并不能通过正常方式访问。

lost+found目录的文件通常是未链接的文件（名字以及被删除），这些文件还被一些进程使用（数据没有删除），在系统突然关机时（内核panic或突然断电）出现。这些文件系统会删除的，你不需要担心。
当因为软件或硬件出现错误，导致文件系统不一致，也有可能把有问题的文件放入到lost+found目录。它提供了恢复丢失文件的一种方法。

如果你不小心删除了lost+found目录，不用使用mkdir命令创建lost+found目录，应该使用 mklost+found命令创建lost+found目录：
$ cd /
sudo mklost+found
```

### Ubuntu14.04

#### 网络，主机名，dns配置：
```
## 问题 1： 修改了网络配置后，重启网络服务失败
/etc/init.d/networking restart
stop: Job failed while stopping
start: Job is already running: networking

## 主机名
在Linux发行版本中，主机名可以存在下面文件中：
/etc/hostname  # ubuntu
/etc/sysconfig/network  # Centos 或 Fedora

/etc/hostname与/etc/hosts的区别
/etc/hostname  # 存放的是主机名
/etc/hosts  存放的是域名与ip的对应关系，域名与主机名没有任何关系

## dns配置
# 临时添加
vim /etc/resolv.conf
nameserver 172.21.5.128

永久配置dns，下面方法之后都可以：
# vim /etc/network/interfaces
dns-nameservers 172.21.5.128

# vim /etc/resolvconf/resolv.conf.d/base
nameserver 192.168.80.2
nameserver 8.8.8.8

```
#### 配置启动项
```
update-rc.d apache2 defaults   ### 添加启动项
update-rc.d -f apache2 remove  ### 删除启动项
```

#### ssh连接慢
```
UseDNS no  ## 增加该选项
GSSAPIAuthentication no   ## 图形方面的认证,默认 yes
```

#### 配置防火墙，关闭
```
1、关闭ubuntu的防火墙
ufw disable
    开启防火墙
ufw enable

2、卸载了iptables
       apt-get remove iptables

3、关闭ubuntu中的防火墙的其余命令
        iptables -P INPUT ACCEPT
        iptables -P FORWARD ACCEPT
        iptables -P OUTPUT ACCEPT
        iptables -F
4、/usr/sbin/sestatus -v      ##如果SELinux status参数为enabled即为开启状态
        SELinux status:                 enabled
5、getenforce                 ##也可以用这个命令检查
        关闭SELinux：
6、临时关闭（不用重启机器）：
        setenforce 0                  ##设置SELinux 成为permissive模式
                                      ##setenforce 1 设置SELinux 成为enforcing模式
7、修改配置文件需要重启机器：
        修改/etc/selinux/config 文件
        将SELINUX=enforcing改为SELINUX=disabled
        重启机器即可
```

#### 配置ntp
```
18 */2 * * * /usr/sbin/ntpdate s2m.time.edu.cn
30 */2 * * * /usr/sbin/ntpdate 172.21.12.68
```

#### lvm 配置
```
### 小于 2T 的磁盘
fdisk /dev/sda  ##  (建立sda3,8e )
mkfs -t ext4 /dev/sda3

### 大于 2T
parted  可以划分单个分区大于2T的GPT格式的分区，也可以划分普通的MBR分区

### 交互操作
parted /dev/sdb
p
mklabel gpt
mkpart
ext4
0T
2T
p
set 1 lvm on
q

### 格式化分区
mkfs.ext4 /dev/sdb1

### lvm 操作
pvs pvdisplay pvcreate ### 查看，创建 Physical volume (pvcreate不是必须)
vgs  vgdisplay  lvscan  ### 查看 Volume group
vgextend  ubuntu14-vg /dev/sdb1  ### 扩充vg (如sdb1不是pv，会自动创建pv,然后添加到vg)

lvcreate -l 524287 ubuntu14-vg -n dbdata  ### 在名为ubuntu14-vg的卷组中创建2T大小的逻辑卷dbdata

### 刷新磁盘信息与写入
#### mkfs.ext4 /dev/ubuntu14-vg/dbdata  ### 新建的lv才需要格式化
resize2fs /dev/ubuntu14-vg/dbdata

### 配置 /etc/fstab  挂载
mkdir -p  /konkacss

/dev/mapper/ubuntu14--vg-dbdata /konkacss    ext4 defaults 0 0

mount -a
```

### centos

#### systemd
```
systemd is not available for EL6, and EL6 is not prepared for systemd. Do not attempt to install systemd on EL6.
If you want to run systemd then install EL7.
```
#### useradd
```
通用配置文件
etc/default/useradd
/etc/login.defs

useradd csstest -d /home/csstest
```

#### sudo 配置
```
表示用户liuxq可以在本机上以root的权限执行"sudo /sbin/poweroff"命令，而不需要root密码（需要liuxq的用户密码）。
如果加上NOPASSWD，则表示不需要输入密码：

liuxq localhost=NOPASSWD: /sbin/poweroff

在配置文件的开头部分，有四个别名类型：
User_Alias; Host_Alias; Runas_Alias; Cmnd_Alias;
User_Alias LIU=liuxq,lxq
定义了一个LIU集合，其中有成员liuxq和lxq
```
