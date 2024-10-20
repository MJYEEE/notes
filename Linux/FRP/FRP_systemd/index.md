# 实验环境
- 客户机：ubuntu20.04
- frp版本：frp_0.54.0_linux_amd64 

# 前排附上官方文档
[官方中文文档](https://gofrp.org/zh-cn/docs/)


# 安装 systemd

在 Linux 系统下，使用 `systemd` 可以方便地控制 frp 的启动、停止、配置后台运行以及开机自启动。

先安装systemd
```shell
apt install systemd
```

# 创建 frpc.service 文件 

在 `/etc/systemd/system` 目录下创建一个 `frpc.service` 文件，用于配置 frpc 服务。
如果你想在服务器端设置，只需要把frpc换成frps即可

```shell
sudo vim /etc/systemd/system/frpc.service
```

写入内容
```ini
[Unit]
# 服务名称，可自定义
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# 启动frps的命令，需修改为您的frps的安装路径
ExecStart = /path/to/frpc -c /path/to/frpc.toml

[Install]
WantedBy = multi-user.target
```

# 重载systemctl
```shell
systemctl daemon-reload
```

# 使用 systemd 命令管理 frps 服务

```shell
# 启动frp
sudo systemctl start frpc
# 停止frp
sudo systemctl stop frpc
# 重启frp
sudo systemctl restart frpc
# 查看frp状态
sudo systemctl status frpc
```

* 如果你在设置systemd之前，已经通过常规命令开启了frpc，请先将其关闭 *

# 设置 frpc 开机自启动

```shell
sudo systemctl enable frpc
```

通过遵循上述步骤，您可以轻松地使用 systemd 来管理 frpc 服务，实现启动、停止、自动运行和开机自启动。确保替换路径和配置文件名称以匹配您的实际安装。
