# FRP 内网穿透简单使用
## 实验环境
- 服务机：ubuntu20.04
- 客户机：ubuntu20.04
- frp版本：frp_0.54.0_linux_amd64 

## 下载frp库
去github下载对应安装包<https://github.com/fatedier/frp/releases>
我这里下载的是**frp_0.54.0_linux_amd64.tar.gz**
下载后解压应该分为以下几个文件
![frp](images/frp_dir.png "frp_dir")
## 双端配置
前排提示，一定不能往配置文件里面加注释，要不然不能用
## 服务机配置
新建文件frps.ini
```shell
vim frps.ini
```
在文件中输入
```shell
[common]
bind_port = 7000
```
以上为最简单的写法
frp监听的端口，默认是7000，可以改成其他的

完成后，输入`./frps -c frps.ini`来开启服务
可以将服务放在screen下或者后台运行，以免退出终端后服务关闭

## 客户机配置
新建文件frpc.ini
```shell
vim frpc.ini
```
在文件中输入
```shell
[common]
server_addr = x:x:x:x # 此处为你的服务机公网IP
server_port = 7000 # 此处为frp监听端口，与服务机配置相同

[project] # 此处填你客户机上要穿透的服务自定义名称，用于区分
local_port = xxxx # 此处为客户机上服务对应端口
remote_port = xxxx # 此处为服务器上穿透端口
```
以上为最简单的写法，复制的话，记得把注释删除掉，可能会出现无法运行的bug
完成后，输入`./frpc -c frpc.ini`来开启服务
可以将服务放在screen下或者后台运行，以免退出终端后服务关闭
