## frp

本地配置

```
serverAddr = "x.x.x.x"
serverPort = 7000

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000
```

服务器配置
```
# vim frps.toml
bindPort = 7000

```

## run
```
./frps -c ./frps.toml
./frpc -c ./frpc.toml
```

## systemd运行
```
apt install systemd
sudo vim /etc/systemd/system/frps.service
```
写入
```
[Unit]
# 服务名称，可自定义
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# 启动frps的命令，需修改为您的frps的安装路径
ExecStart = /path/to/frps -c /path/to/frps.toml

[Install]
WantedBy = multi-user.target

```

运行指令
```
# 启动frp
sudo systemctl start frps
# 停止frp
sudo systemctl stop frps
# 重启frp
sudo systemctl restart frps
# 查看frp状态
sudo systemctl status frps

```
开机自启动
```
sudo systemctl enable frps

```

## 连接
```
ssh -o Port=6000 localusername@x.x.x.x
# or
ssh -p 6000 localusername@x.x.x.x
```

## note
公网服务器要开启6000和7000两个端口
