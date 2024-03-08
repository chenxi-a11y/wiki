# Docker部署Hysteria2后端

系统Debian12，安装Docker：

```
apt -y update
apt -y install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

新建目录以及docker-compose文件：

```
mkdir -p /opt/hysteria-server && cd /opt/hysteria-server && nano docker-compose.yml
```

写入如下配置，重要的部分都写了注释：

```
version: '3.8'
services:
  certbot:
    image: certbot/dns-cloudflare:latest
    container_name: certbot
    restart: unless-stopped
    volumes:
      - certbot_etc:/etc/letsencrypt
      - certbot_var:/var/lib/letsencrypt
      - ./cloudflare.ini:/etc/cloudflare.ini
    command: >-
      certonly
      --dns-cloudflare
      --dns-cloudflare-credentials /etc/cloudflare.ini // 存放CloudFlare的API密钥文件
      --email example@lala.im // 你的邮箱
      --agree-tos
      --no-eff-email
      -d *.example.com // 要申请的域名，*代表通配符，多个域名可以使用多个-d参数来指定

  hysteria:
    image: ghcr.io/cedar2025/hysteria:latest
    container_name: hysteria
    restart: unless-stopped
    network_mode: "host" // 使用主机网络
    volumes:
      - certbot_etc:/etc/letsencrypt
      - ./server.yaml:/etc/hysteria/server.yaml

volumes:
  certbot_etc:
  certbot_var:
```

在/opt/hysteria-server目录下新建cloudflare.ini配置文件：

```
nano cloudflare.ini
```

写入如下配置：

```
dns_cloudflare_api_token = example
```

CloudFlareAPI申请方法：https://developers.cloudflare.com/fundamentals/api/get-started/create-token/

在/opt/hysteria-server目录下新建server.yaml配置文件：

```
nano server.yaml
```

写入如下配置：

```
v2board:
  apiHost: https://xboard.example.com // Xboard面板地址
  apiKey: panelkey // Xboard面板内设置的通讯密钥
  nodeID: 1 // Xboard面板内的节点ID
tls:
  type: tls
  cert: /etc/letsencrypt/live/example.com/fullchain.pem // 证书路径
  key: /etc/letsencrypt/live/example.com/privkey.pem // 私钥路径
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

先申请SSL证书：

```
docker compose run --rm certbot
```

续签证书可用下面的命令：

```
docker compose run --rm certbot renew
```

然后启动后端：

```
docker compose up -d hysteria
```

默认情况下，后端会监听在443端口，直到你在面板上添加好了对应的节点，重启后端生效：

```
docker compose restart hysteria
```