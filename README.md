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
制作镜像前根据自己的需求调整值，具体的值请查看各文件夹下的Dockerfile内容。
执行如下脚本
```sh
#制作mysql自定义镜像
docker build -t=ycftest/lnmp:mysql ./mysql/
#制作php自定义镜像
docker build -t=ycftest/lnmp:php ./php/
#制作nginx自定义镜像
docker build -t=ycftest/lnmp:nginx ./nginx/
```
此叫笨将会

###第四步、启动容器
执行如下脚本
```sh
#创建mysql数据文件目录
mkdir -p /data/mysql
#启动mysql容器
docker run --name mysql -v /data/mysql:/var/lib/mysql -p 3306:3306 -d zhaojianhui/lnmp:mysql


```

###第五步、绑定host
