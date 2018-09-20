### centos7 安装
[Centos7安装RabbitMQ](http://www.jianshu.com/p/4c8a96f0467b)
#### erlang安装
  - 下载地址 http://www.rabbitmq.com/releases/erlang/
  rpm -ivh erlang-19.0.4-1.el7.centos.x86_64.rpm

#### rabbitmq-server 安装
    - 下载地址 http://www.rabbitmq.com/install-rpm.html
    wget https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.14-1.el7.noarch.rpm
    wget https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.14-1.el7.noarch.rpm.asc

    rpm --import rabbitmq-server-3.6.14-1.el7.noarch.rpm.asc
    yum install -y rabbitmq-server-3.6.14-1.el7.noarch.rpm

#### 启动服务
  - vim /etc/hosts，添加一行：127.0.0.1 主机名

  - 启动：service rabbitmq-server start
  - 停止：service rabbitmq-server stop
  - 重启：service rabbitmq-server restart
  - 设置开机启动：chkconfig rabbitmq-server on

#### 配置
  复制默认配置：cp /usr/share/doc/rabbitmq-server-3.6.10/rabbitmq.config.example /etc/rabbitmq/
  修改配置文件名：cd /etc/rabbitmq ; mv rabbitmq.config.example rabbitmq.config
  编辑配置文件，开启用户远程访问：vim rabbitmq.config
```
  rabbit, 配置:
    {tcp_listeners,[8489]},
    {loopback_users,["admin"]}
    新增admin用户
    rabbitmqctl add_user admin admin
    rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"
    rabbitmqctl set_user_tags admin administrator

  rabbitmq_management, 配置：
    {listener,
      [
              {port,     8488},
              {ip,       "0.0.0.0"},
              {ssl,     false}
      ]}

### 查看用户列表
rabbitmqctl list_users

### 查看权限
rabbitmqctl list_permissions -p
```
#### 开启 Web 界面管理
  rabbitmq-plugins enable rabbitmq_management
  重启 RabbitMQ 服务：service rabbitmq-server restart
  浏览器访问：http://192.168.x.xxx:8488
    默认管理员账号：guest 默认管理员密码：guest

### 旧版 centos6 安装
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
yum install -y erlang


wget http://www6.atomicorp.com/channels/atomic/centos/6/x86_64/RPMS/socat-1.7.2.1-2.el6.art.x86_64.rpm
rpm -Uvh socat-1.7.2.1-2.el6.art.x86_64.rpm
#yum install -y socat


curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
yum install -y rabbitmq-server


chkconfig rabbitmq-server on
/sbin/service rabbitmq-server start

# 安装Web管理界面插件
rabbitmq-plugins enable rabbitmq_management

# Add a new/fresh user, say user ‘test’ and password ‘test’
rabbitmqctl add_user admin admin
# Give administrative access to the new access
rabbitmqctl set_user_tags admin administrator
# Set permission to newly created user
rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"


#### 集群配置
scp /etc/hosts 172.21.12.81:/etc/hosts
scp /etc/hosts 172.21.12.82:/etc/hosts

scp /var/lib/rabbitmq/.erlang.cookie 172.21.12.81:/var/lib/rabbitmq/.erlang.cookie
scp /var/lib/rabbitmq/.erlang.cookie 172.21.12.82:/var/lib/rabbitmq/.erlang.cookie

chmod 400 /var/lib/rabbitmq/.erlang.cookie
chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie

# 使用 -detached 参数运行各节点
rabbitmqctl stop
rabbitmq-server -detached

rabbitmqctl cluster_status


rabbitmqctl stop_app
rabbitmqctl join_cluster rabbit@mj92
rabbitmqctl start_app


http://www.jianshu.com/p/db0f5496f0d2
http://www.cnblogs.com/lion.net/p/5725474.html





################################################################
################################################################
92 cluster
启动线程数:【100】	每个线程发送消息数:【1000】	发送消息包大小:【100 byte】
发送消息处理时间:【2099209 ms】	处理发送消息速度(QPS):每秒【47 次】	发送消息的平均时间:【20 ms】

63
启动线程数:【100】	每个线程发送消息数:【1000】	发送消息包大小:【100 byte】
发送消息处理时间:【1171743 ms】	处理发送消息速度(QPS):每秒【85 次】	发送消息的平均时间:【11 ms】

92:8005 cluster测试
启动线程数:【10】	每个线程发送消息数:【5000】	发送消息包大小:【1000 byte】
发送消息处理时间:【206868 ms】	处理发送消息速度(QPS):每秒【241 次】	发送消息的平均时间:【4 ms】

92:8005 单机测试
启动线程数:【10】	每个线程发送消息数:【5000】	发送消息包大小:【1000 byte】
发送消息处理时间:【60325 ms】	处理发送消息速度(QPS):每秒【828 次】	发送消息的平均时间:【1 ms】


启动线程数:【16】	每个线程发送消息数:【10000】	发送消息包大小:【1000 byte】
发送消息处理时间:【837960 ms】	处理发送消息速度(QPS):每秒【190 次】	发送消息的平均时间:【5 ms】


启动线程数:【16】	每个线程发送消息数:【10000】	发送消息包大小:【1000 byte】
发送消息处理时间:【596996 ms】	处理发送消息速度(QPS):每秒【268 次】	发送消息的平均时间:【3 ms】


Message counts.

Note that "in memory" and "persistent" are not mutually exclusive; persistent messages can be in memory as well as on disc, and transient messages can be paged out if memory is tight. Non-durable queues will consider all messages to be transient.


rabbitmqctl stop
rabbitmq-server -detached

rabbitmqctl stop_app
rabbitmqctl join_cluster rabbit@mj92
rabbitmqctl start_app

rabbitmqctl cluster_status
