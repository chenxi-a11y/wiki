# X-ui结合上篇Clash解锁流媒体

```
//教程和X-ui使用二级代理解锁流媒体一样我们只需要在修改配置节点那一步 配置节点换下面这一个

//二级代理
//填入你自己的解锁节点配置
{
      "tag": "netflix_proxy",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "127.0.0.1", //这里127.0.0.1代表本机ip
            "ota": false,
            "port": 7891, //7891代表config.yaml中socks端口
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
```

