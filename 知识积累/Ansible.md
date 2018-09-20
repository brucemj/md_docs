
### 安装ansible在 ubuntu14.04

#### 安装software-properties-common包
```
sudo apt-get update
sudo apt-get install software-properties-common
```

#### 添加Ansible PPA
```
sudo apt-add-repository ppa:ansible/ansible
```

#### 安装软件
```
sudo apt-get update
sudo apt-get install ansible
```

### 基本使用
#### 配置hosts 和 简单命令
```
vim /etc/ansible/hosts
[r730]
172.21.12.120
172.21.12.122
172.21.12.123
172.21.12.124
172.21.12.126

ansible r730 -a 'free -m' -u root
```

#### 统计homedisk
ansible all -a 'wget  http://172.21.5.142/tools/build/hometest.sh -O /opt/hometest.sh' -u root
ansible all -a 'wget  http://172.21.5.142/tools/build/homedisk.sh -O /opt/homedisk.sh' -u root
ansible all -a 'wget  http://172.21.5.142/tools/build/hostinfo.sh -O /opt/hostinfo.sh' -u root

ansible all -a 'wget  http://172.21.5.142/tools/build/rt.sh -O /opt/rt.sh' -u root

ansible all -a 'wget  http://172.21.5.142/tools/build/shutdown.sh -O /opt/shutdown.sh' -u root




ansible all -a 'at now + 1 minutes -f /opt/hometest.sh' -u root
ansible all -a 'at 02:30 12/13/2017 -f /opt/homedisk.sh' -u root

ansible hp -a 'at 08:58 12/08/2017 -f /opt/homedisk.sh' -u root

ansible disk -a 'at 07:50 12/11/2017 -f /opt/rt.sh' -u root

ansible disk -a 'at 22:45 12/11/2017 -f /opt/shutdown.sh' -u root


ansible disk -a 'at 22:45 12/11/2017 -f /opt/shutdown.sh' -u root
```
## 重启
wget  http://172.21.5.142/tools/build/rt.sh -O /opt/rt.sh
at 10:20 12/09/2017 -f /opt/rt.sh
```
####
