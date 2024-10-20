# FRP 内网穿透简单使用
---

# 实验环境
- 服务机：ubuntu20.04
- 客户机：ubuntu20.04
- frp版本：frp_0.54.0_linux_amd64 

# 前排附上官方文档
[官方中文文档](https://gofrp.org/zh-cn/docs/)

# 下载frp库
去github下载对应安装包<https://github.com/fatedier/frp/releases>
我这里下载的是**frp_0.54.0_linux_amd64.tar.gz**
下载后解压应该分为以下几个文件
![frp](images/frp_dir.png)

---

从 v0.52.0 版本开始，frp 开始支持 TOML、YAML 和 JSON 作为配置文件格式。

请注意，INI 已被弃用（虽然现在也能用），并将在未来的发布中移除。新功能只能在 TOML、YAML 或 JSON 中使用。

建议使用 TOML 作为配置文件格式。

---

# 双端配置
## 服务机配置
新建文件frps.toml
```shell
vim frps.toml
```
在文件中输入

```toml
bindPort = 7000
```
以上为最简单的写法，frp监听的端口，默认是7000，一般建议改成其他的。

完成后，输入`./frps -c frps.toml`来开启服务，可以将服务放在screen下或者后台运行，以免退出终端后服务关闭

## 客户机配置
新建文件frpc.toml
```shell
vim frpc.toml
```
在文件中输入
```toml
serverAddr = "x.x.x.x" # 此处为你的服务机公网IP
serverPort = 7000 # 此处为frp监听端口，与服务机配置

[[proxies]] # 代理配置
name = "xxx" # 代理名称
type = "tcp" # 代理类型
localIP = "127.0.0.1" # 被代理的本地服务 IP
localPort = xxx # 被代理的本地服务端口
remotePort = xxx # 服务端绑定的端口
```
以上为最简单的写法，复制的话，记得把注释删除掉，可能会出现无法运行的bug
完成后，输入`./frpc -c frpc.toml`来开启服务
可以将服务放在screen下或者后台运行，以免退出终端后服务关闭
