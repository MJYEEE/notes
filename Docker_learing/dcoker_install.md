# docker 安装
本文主要介绍docker的安装教程，由于目前网络环境影响，这里介绍的是离线安装方法
实验环境为ubuntu20.04

## 前排提醒
- 如果是刚安装的系统请先更新索引`sudo apt update` 
- 如果之前有安装过docker的，请先卸载旧版本`sudo apt-get remove docker docker-engine docker.io containerd runc`

## 下载安装包
到 https://download.docker.com/linux/ubuntu/dists/ 去下所需软件包.
进入网站依次点击 focal/ ---> pool/ ---> stable/,接着根据自己电脑选择合适的架构,这里选择amd64/
接着下载以下软件包
- containerd.io_<version>_<arch>.deb
- docker-ce-cli_<version>_<arch>.deb
- docker-ce_<version>_<arch>.deb
- docker-buildx-plugin_<version>_<arch>.deb
- docker-compose-plugin_<version>_<arch>.deb

上面三个是必须的,下面两个看自己需求安装,这里只安装上面三个.
我选择的版本依次为
- containerd.io_1.7.21-1_amd64.deb  
- docker-ce-cli_27.2.0-1~ubuntu.20.04~focal_amd64.deb   
- docker-ce_27.2.0-1~ubuntu.20.04~focal_amd64.deb   

下载完成之后,将安装包拷贝到服务器上

## 服务器安装
拷贝完所有安装包之后,需要依次安装软件(顺序不能错)
依次为 containerd.io --> docker-ce-cli --> docker-ce
```
sudo dpkg -i containerd.io_1.4.12_amd64.deb
sudo dpkg -i docker-ce-cli_20.10.12_amd64.deb
sudo dpkg -i docker-ce_20.10.12_amd64.deb
```

## 验证安装
启动docker `sudo systemctl start docker` 
查看docker信息 `sudo docker --version` 
如果有信息,则安装成功

## 简单设置添加用户组
想要在非root权限在使用docker,还需要将当前用户添加到docker用户组里面
```
sudo groupadd docker
sudo gpasswd -a ${USER} docker
sudo service docker restart
newgrp - docker
```
注意:最后一步是必须的，否则因为 groups 命令获取到的是缓存的组信息，刚添加的组信息未能生效，所以 docker images 执行时同样有错。