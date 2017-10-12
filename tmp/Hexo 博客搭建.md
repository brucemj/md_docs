Hexo 博客





### [安装 docker 在 Centos6](http://blog.csdn.net/kinginblue/article/details/73527832)
- [How To Install Docker on CentOS 6 ](https://www.liquidweb.com/kb/how-to-install-docker-on-centos-6/)

rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

rpm -iUvh  epel-release-6-8.noarch.rpm
yum update -y
yum -y install docker-io
service docker start
chkconfig docker on





### [基于docker的hexo博客系统](https://dev.aliyun.com/detail.html?spm=5176.1972343.2.52.UlIt1y&repoId=32124)

docker pull registry.cn-hangzhou.aliyuncs.com/wuhulala/website
docker run -p 4000:4000 --name website -v /opt/hexo:/opt/website wuhulala/website
