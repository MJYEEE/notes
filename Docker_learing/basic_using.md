# docker的基础用法
---

## docker hub登录
```
docker login  #登录输入帐号和密码 
```
```
docker logout #退出登录
```

## 搜索镜像
```
docker search ubuntu
```

## 获取镜像
```
docker pull ubuntu
```

## 启动容器
```
docker run -it ubuntu /bin/bash
```
- -i: 交互式操作。
- -t: 终端。
- ubuntu: ubuntu 镜像。
- /bin/bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。

输入exit退出容器

##进入容器
```
docker exec -it ubuntu /bin/bash
```

## 将本地文件复制到容器中
```
docker cp xxxx ubuntu:xxxx
```
将当前目录下的xxxx文件复制到ubunut容器下的xxxx位置
- 在xxxx前面加上-r，即可复制文件夹


将两者倒置即可从容器中复制文件到本地目录
```
docker cp ubuntu:xxxx xxxx 
```

## 比复制文件更方便的是挂载本地文件夹
```
docker run -it -v /home/dock/Downloads:/usr/Downloads ubuntu64 /bin/bash
```
通过-v参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径。
现在镜像内就可以共享宿主机里的文件了。
默认挂载的路径权限为读写。如果指定为只读可以用：ro
```
docker run -it -v /home/dock/Downloads:/usr/Downloads:ro ubuntu64 /bin/bash
```