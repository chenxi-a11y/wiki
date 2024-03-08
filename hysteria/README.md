# hysteria后端对接Xb面板

项目地址：https://github.com/cedar2025/Xboard

配套后端：https://github.com/cedar2025/hysteria



Xboard后台添加节点，节点IP填服务器的公网IP、节点地址填写域名、连接端口和服务端口保持一致、协议版本选择v2：

[![img](./tp/README.tp/lala.im_2024-02-25_17-05-00.png)](https://lala.im/wp-content/uploads/2024/02/lala.im_2024-02-25_17-05-00.png)

## 安装后端

[使用后端](https://github.com/cedar2025/hysteria)

准备一个域名，解析到后端节点服务器，本文示例：xbhy2.example.com

80端口不能被占用，Certbot申请证书需要用到。

系统Debian12，首先安装需要用到的包：

```
apt -y update
apt -y install curl git python3 certbot
```

安装Golang：

```
curl -L https://go.dev/dl/go1.21.7.linux-amd64.tar.gz -o go1.21.7.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.21.7.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' > /etc/profile.d/golang.sh
source /etc/profile.d/golang.sh
```

获取后端源码：

```
git clone https://github.com/cedar2025/hysteria.git
cd hysteria/
```

编译：

```
python3 hyperbole.py build -r
```



完成后build目录内就是可用的二进制文件了，复制一份：

```
cp build/hysteria-linux-amd64 /usr/local/bin
```



新建配置文件：

```
mkdir -p /etc/hysteria && nano /etc/hysteria/config.yaml
```

写入如下配置：

```
v2board:
  apiHost: https://xboard.example.com // Xboard面板地址
  apiKey: panelkey // Xboard面板内设置的通讯密钥
  nodeID: 1 // Xboard面板内的节点ID
tls:
  type: tls
  cert: /etc/letsencrypt/live/xbhy2.example.com/fullchain.pem // Certbot申请的证书
  key: /etc/letsencrypt/live/xbhy2.example.com/privkey.pem // Certbot申请的证书私钥
auth:
  type: v2board
trafficStats:
  listen: 127.0.0.1:7653
acl:
  inline: 
    - reject(10.0.0.0/8)
    - reject(172.16.0.0/12)
    - reject(192.168.0.0/16)
    - reject(127.0.0.0/8)
    - reject(fc00::/7)
```



申请SSL证书：

```
certbot certonly
```



新建Systemd服务：

```
nano /etc/systemd/system/hysteria-server.service
```

写入如下配置：

```
[Unit]
Description=Hysteria Server
After=network.target

[Service]
Type=simple
WorkingDirectory=~
ExecStart=/usr/local/bin/hysteria-linux-amd64 server -c /etc/hysteria/config.yaml
Environment=HYSTERIA_LOG_LEVEL=info
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```



启动并设置开机自启：

```
systemctl enable --now hysteria-server
```



其它命令：

```
#启动hysteria
systemctl start hysteria-server.service
#停止hysteria
systemctl stop hysteria-server.service
#重启hysteria
systemctl restart hysteria-server.service
#查看hysteria运行状态
systemctl status hysteria-server.service

#设置hysteria开机自启动
systemctl enable hysteria-server
#或
systemctl enable hysteria-server.service

#赋予执行权限
chmod +x /usr/local/bin/hysteria-linux-amd64

#重新加载systemctl daemon
systemctl daemon-reload
```



## 直接使用编译好的hysteria

首先安装需要用到的包：

```
apt -y update
apt -y install curl git python3 certbot
```



安装Golang：

```
curl -L https://go.dev/dl/go1.21.7.linux-amd64.tar.gz -o go1.21.7.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.21.7.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' > /etc/profile.d/golang.sh
source /etc/profile.d/golang.sh
```



申请SSL证书：

```
certbot certonly
```



把编译好的 hysteria-linux-amd64 文件 上传都 /usr/local/bin文件夹

把我们etc里的文件进行修改，然后上传到服务器对应目录

启动并设置开机自启：

```
systemctl enable --now hysteria-server
```

