# FRP 内网穿透简单使用

### 下载frp库
https://github.com/fatedier/frp/releases
下载后解压应该分为以下几个文件
![frp](images/frp_dir.png "frp_dir")
### 双端配置
前排提示，一定不能往配置文件里面加注释，要不然不能用
#### 服务机配置
新建文件frps.ini
```vi frps.ini```
在文件中输入
```
[common]
bind_port = 7000
```
以上为最简单的写法
frp监听的端口，默认是7000，可以改成其他的

完成后，输入`./frps -c frps.ini`来开启服务
可以将服务放在screen下或者后台运行，以免退出终端后服务关闭

#### 客户机配置
新建文件frpc.ini
```vi frpc.ini```
在文件中输入
```
[common]
server_addr = x:x:x:x # 此处为你的服务机公网IP
server_port = 7000 # 此处为frp监听端口，与服务机配置相同

[project] # 此处填你客户机上要穿透的服务自定义名称，用于区分
local_port = xxxx # 此处为客户机上服务对应端口
remote_port = xxxx # 此处为服务器上穿透端口
```
以上为最简单的写法
完成后，输入`./frpc -c frpc.ini`来开启服务
可以将服务放在screen下或者后台运行，以免退出终端后服务关闭
