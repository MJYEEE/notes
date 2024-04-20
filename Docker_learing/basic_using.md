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