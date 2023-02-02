# XrayR二级代理解锁流媒体

## 1.XrayR二级代理解锁流媒体

**修改config.yml配置**

```
RouteConfigPath: /etc/XrayR/route.json
OutboundConfigPath: /etc/XrayR/custom_outbound.json
```

**添加路由规则**

/etc/XrayR/route.json

修改成下面这样

```
    {
    "type": "field",
    "outboundTag": "netflix_proxy",
    "domain": [
        "geosite:netflix",
        "geosite:disney"
    ]
},
    {
      "type": "field",
      "outboundTag": "block",
      "ip": [
        "geoip:private"
      ]
    },
    {
      "type": "field",
      "outboundTag": "block",
      "protocol": [
        "bittorrent"
      ]
    },
    {
      "type": "field",
      "outboundTag": "IPv4_out",
      "network": "udp,tcp"
    }
  ]
}
```

**二级代理 //填入你自己的解锁节点配置**

/etc/XrayR/custom_outbound.json

```
  {
    "tag": "IPv6_out",
    "protocol": "freedom",
    "settings": {
      "domainStrategy": "UseIPv6"
    }
  },
  {
      "tag": "netflix_proxy",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "172.104.118.92",
            "ota": false,
            "port": 47505,
            "level": 1
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      },
      "mux": {
        "enabled": false,
        "concurrency": -1
      }
    },
  {
    "protocol": "blackhole",
    "tag": "block"
  }
]
```

**重启 XrayR**

**到这里二级代理解锁流媒体就结束了**

## 2.XrayR全局二级代理

**添加路由规则**

/etc/XrayR/route.json

修改成下面这样

```
{
  "domainStrategy": "IPOnDemand",
  "rules": [
    {
      "type": "field",
      "outboundTag": "block",
      "ip": [
        "geoip:private"
      ]
    },
    {
      "type": "field",
      "outboundTag": "block",
      "protocol": [
        "bittorrent"
      ]
    },
    {
      "type": "field",
      "outboundTag": "IPv4_out",
      "network": "udp,tcp"
    }
  ]
}
```

**二级代理 //填入你自己的解锁节点配置**

/etc/XrayR/custom_outbound.json

```
[
  {
      "tag": "IPv4_out",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "jjs2.ddnfdsnfj.ga",
            "port": 80,
            "users": [
              {
                "id": "b3a8d0df-4ea4-4772-935e-051e06653ea2",
                "alterId": 0,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/",
          "headers": {
            "Host": "tms.dingtalk.com"
          }
        }
      },
      "mux": {
        "enabled": false,
        "concurrency": -1
      }
    },
  {
    "tag": "IPv6_out",
    "protocol": "freedom",
    "settings": {
      "domainStrategy": "UseIPv6"
    }
  },
  {
    "protocol": "blackhole",
    "tag": "block"
  }
]
```

**重启 XrayR**

