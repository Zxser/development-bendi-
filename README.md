# ycfDocker
使用docker在虚拟机上部署本地开发环境。本教程使用centos操作系统搭建环境，如有不成功的地方，请按实际情况执行。
>[中文文档地址](https://yeasy.gitbooks.io/docker_practice/content/index.html)


###第一步、准备
工具：VMware Workstation（虚拟机）、SecureCRT（终端仿真程序）
虚拟机准备：centos需要安装vmware tools，虚拟机需要开通文件夹共享并且将项目目录共享到操作系统的/mnt/hgfs/GIT/目录。
> vmware tool安装方法 [点击查看](https://github.com/zhaojianhui129/lnmp/blob/master/vmware%20tool%E5%AE%89%E8%A3%85.md)

在centos上安装好git工具，到时候拉取代码需要在这里执行。
```sh
yum install git -y
```


###第二步、安装Docker
安装方法点击：[链接](https://github.com/zhaojianhui129/docker/blob/master/centos7%E4%B8%8B%E5%AE%89%E8%A3%85%E6%9C%80%E6%96%B0%E7%89%88%E7%9A%84docker.md)
安装完成docker之后注册docker帐号，使用如下命令快速注册官方仓库帐号：
```sh
docker login -u ycftest -p 123456 -e 'zhaojianhui@yaochufa.com'
```
然后提示需要验证邮箱，验证成功之后创建名称为lnmp（可自定义）仓库。
最后使用一下命令登录，
```sh
docker login -u ycftest -p 123456
```

###第三步、制作自定义镜像
拉取ycfDocker项目列表

```sh
#克隆代码
git clone 'https://github.com/zhaojianhui129/ycfDocker.git'
#拉取更新代码
cd ycfDocker/
git pull origin master
```

制作镜像前根据自己的需求调整值，具体的设置值请查看各文件夹下的Dockerfile内容。
执行以下命令，因为要从官方仓库拉取镜像，速度会很缓慢，耐心等待，并且多次尝试。
直到出现（Successfully built b4dbab3c9442）的字样才算成功。
```sh
#制作mysql自定义镜像
docker build -t=ycftest/lnmp:mysql ./mysql/
#制作php自定义镜像
docker build -t=ycftest/lnmp:php ./php/
#制作nginx自定义镜像
docker build -t=ycftest/lnmp:nginx ./nginx/
#拉取ubuntu镜像用来制作数据卷容器
docker pull ubuntu
```

> 有效的php扩展列表为:
> bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext gmp hash iconv imap interbase intl json ldap mbstring mcrypt mysqli oci8 odbc opcache pcntl pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell readline recode reflection session shmop simplexml snmp soap sockets spl standard sysvmsg sysvsem sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zip

官方仓库地址：
> php
> [英文地址](https://hub.docker.com/_/php/) 
> [中文地址](https://github.com/DaoCloud/library-image/tree/master/php)
> <br>
> mysql
> [英文地址](https://hub.docker.com/_/mysql/)
> [中文地址](https://github.com/DaoCloud/library-image/tree/master/mysql)
> <br>
> nginx
> [英文地址](https://hub.docker.com/_/nginx/)
> [中文地址](https://github.com/DaoCloud/library-image/tree/master/nginx)

###第四步、启动容器
执行如下脚本
```sh
#创建mysql数据文件目录
mkdir -p /data/mysql
#启动mysql容器
docker run --name mysql -v /data/mysql:/var/lib/mysql -p 3306:3306 -d ycftest/lnmp:mysql

#将共享主机的目录挂载成一个数据卷容器，此容器用于共享php和nginx代码
docker run -d -v /mnt/hgfs/GIT/:/www-data/ --name web ubuntu echo Data-only container for postgres
#启动php容器
docker run --name php --volumes-from web -d ycftest/lnmp:php
#启动nginx容器
docker run --name nginx --volumes-from web  -p 80:80 --link php:php -d ycftest/lnmp:nginx

```

###第五步、绑定host

添加以下host到window目录的C:\Windows\System32\drivers\etc\HOSTS文件中，记住下面ip地址改为虚拟机ip。
```sh
192.168.10.101 mtest.yaochufa.com
192.168.10.101 youtest.yaochufa.com
192.168.10.101 acttest.yaochufa.com
192.168.10.101 apitest.yaochufa.com
192.168.10.101 apidoctest.yaochufa.com
192.168.10.101 jobtest.yaochufa.com
192.168.10.101 notifytest.yaochufa.com
192.168.10.101 cpstest.yaochufa.com
192.168.10.101 weixintest.yaochufa.com
192.168.10.101 wwwtest.yaochufa.com
```

####第六步、打开浏览器开始查看吧。