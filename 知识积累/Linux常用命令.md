
#### sh 和 bash 的区别
- 因为 at 默认使用的 sh shell 运行脚本，所有脚本编写需要遵循sh和bash格式
```
## at 使用 /bin/sh 变量计算和 bash 不太一样
cpunum=$(cat /proc/cpuinfo | grep processor | wc -l)
#((tnum=${cpunum}*5/12))  bash正常
tnum=$(echo $cpunum*5/12 | bc)

## $? if条件判断
if [ $? -eq 0 ]; then

## 函数定义
fun()
{

}


```

#### at
- /etc/init.d/atd restart  ## 需要首先检查atd服务是否启动
```
## at命令用于在指定时间执行命令。at允许使用一套相当复杂的指定时间的方法。
## 选项
-f：指定包含具体指令的任务文件；
-c: 查看job
-q：指定新任务的队列名称；
-l：显示待执行任务的列表；
-d：删除指定的待执行任务；
-m：任务执行完成后向用户发送E-mail。

## 示例
at now + 1 minutes   ## 任务在5分钟后运行
at 5pm+3 days -f job.sh  ##三天后的下午 5
at 17:20 tomorrow -f job.sh  ## 明天17点钟
at 23:59 12/31/2018 -f job.sh ## 具体某一天

## atq命令显示系统中待执行的任务列表，也就是列出当前用户的at任务列表。
atq

## atrm命令用于删除待执行任务队列中的指定任务。
atrm 2

## at 使用 /bin/sh 变量计算和 bash 不太一样
cpunum=$(cat /proc/cpuinfo | grep processor | wc -l)
#((tnum=${cpunum}*5/12))  bash正常
tnum=$(echo $cpunum*5/12 | bc)



```

#### 时区设置
- 时区配置文件
  - /etc/localtime

- tzselect 可查看并选择已安装的时区文件。

- export TZ='Asia/Shanghai'  ## 改变当前用户的时区
- source ~/.bashrc    ## 执行完成之后需要重新登录系统或刷新 ~/.bashrc 生效。

- 更改Linux系统时区
  - sudo rm -rf /etc/localtime
    - 删除后，时区变为 UTC ，比北京时间慢8个小时
  - ln -s /usr/share/zoneinfo/Asia/Hong_Kong /etc/localtime

#### sed
```
sed -i "s/\"threads\".*$/\"threads\" : \"${tnum}\"/g" json
```

#### ps
```
ps -ef 可以看到父进程，对于某些死循环脚本比较有用，可以直接杀死父进程
```

#### ssh 转发隧道，反向连接
- https://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/index.html

#### 用户锁定和解锁
```
## passwd 命令
passwd -l lxj    --- -l 锁定
Locking password for user lxj.
passwd: Success
passwd -S lxj    --- 查看状态
lxj LK 2016-06-20 0 99999 7 -1 (Password locked.)
passwd -u lxj    --- 解锁
Unlocking password for user lxj.
passwd: Success
passwd -S lxj
lxj PS 2016-06-20 0 99999 7 -1 (Password set, SHA512 crypt.)

## usermod 命令
usermod -L lxj  # 锁定
usermod -U lxj  # 解锁
```

#### tar.xz 文件解压
```
$xz -d ***.tar.xz
$tar -xvf  ***.tar
```


#### find
```
## 查找比 2016-11-25 新的文件
find -name Dbid.ini -newermt '2016-11-25 08:00:00'

```
