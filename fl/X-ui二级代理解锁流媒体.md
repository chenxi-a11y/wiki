# X-ui使用二级代理解锁流媒体

**检测是否解锁奈飞：**

```shell
#项目地址：https://github.com/sjlleo/netflix-verify

#下载检测解锁程序
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/v3.1.0/nf_linux_amd64 && chmod +x nf

#执行
./nf

#通过代理执行
./nf -proxy socks5://127.0.0.1:30000
```

**视频教程**https://www.aliyundrive.com/s/En5KWK7zWta 

```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)

国内用这个：
yum install wget -y && wget www.23ll.cn/xui/xui.sh && bash xui.sh
```

## 2.配置模板

### 要用到的配置

```
//本地监听配置
{
    "listen": "127.0.0.1",
    "port": 30000, 
    "protocol": "socks", 
    "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
    }
}

//路由规则
{
    "type": "field",
    "outboundTag": "netflix_proxy",
    "domain": [
        "geosite:netflix",
        "geosite:disney"
    ]
}

//二级代理
//填入你自己的解锁节点配置
//win打开V2rayN→选择节点→右击鼠标→导出所选服务器为客户端配置
{
      "tag": "cn1",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "212.192.15.80",
            "port": 80,
            "users": [
              {
                "id": "1b94f603-7f5a-4c23-9f46-546e0d6e1859",
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
            "Host": "a.189.cn"
          }
        }
      },
      "mux": {
        "enabled": false,
        "concurrency": -1
      }
    }
```

### 面板设置→xray相关设置 x-ui自带配置

```
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      },
      "tag": "api"
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "policy": {
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
    "rules": [
      {
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
      },
      {
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked",
        "type": "field"
      },
      {
        "outboundTag": "blocked",
        "protocol": [
          "bittorrent"
        ],
        "type": "field"
      }
    ]
  },
  "stats": {}
}
```

## 修改开始

```
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
//第一步修改本地监听配置
    {
    "listen": "127.0.0.1",
    "port": 30000, 
    "protocol": "socks", 
    "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
    }
}
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
//第三步填入你自己的解锁节点配置
	{
      "tag": "netflix_proxy", //修改tag与添加的路由tag一样
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "212.192.15.80",
            "port": 80,
            "users": [
              {
                "id": "1b94f603-7f5a-4c23-9f46-546e0d6e1859",
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
            "Host": "a.189.cn"
          }
        }
      },
      "mux": {
        "enabled": false,
        "concurrency": -1
      }
    }, //注意别忘记添加后面的,号
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "policy": {
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
    "rules": [
//第二步添加路由规则
    {
    "type": "field",
    "outboundTag": "netflix_proxy",
    "domain": [
        "geosite:netflix",
        "geosite:disney"
//可以添加其他域名组，域名组大全:https://github.com/v2fly/domain-list-community/tree/master/data
    ]
},
      {
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
      },
      {
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked",
        "type": "field"
      },
      {
        "outboundTag": "blocked",
        "protocol": [
          "bittorrent"
        ],
        "type": "field"
      }
    ]
  },
  "stats": {}
}
```

**保存配置之后一定要重启面板**
