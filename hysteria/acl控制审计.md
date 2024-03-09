## 使用acl控制审计

```
v2board:
  apiHost: http://23.139.25.2:81
  apiKey: dhrdkredsekj45753fdk
  nodeID: 1
tls:
  type: tls
  cert: /etc/letsencrypt/live/uscs.o8p.me/fullchain.pem
  key: /etc/letsencrypt/live/uscs.o8p.me/privkey.pem
auth:
  type: v2board
trafficStats:
  listen: 127.0.0.1:7653
outbounds:
  - name: direct
    type: direct
    direct:
      mode: auto
  - name: proxy
    type: socks5
    socks5:
      addr: 127.0.0.1:40000

acl:
  inline: 
    - reject(10.0.0.0/8)
    - reject(172.16.0.0/12)
    - reject(192.168.0.0/16)
    - reject(127.0.0.0/8)
    - reject(fc00::/7)
    - proxy(geosite:netflix)
    - proxy(geosite:disney)
    - proxy(geosite:youtube)
    - proxy(geosite:primevideo)
    - proxy(geosite:openai)
    - reject(www.360.com)
    - reject(sinaimg.com)
    - direct(all)
```

**- reject**  屏蔽指定域名

**- proxy**  流媒体解锁 （可配合WARP  WARP脚本推荐：[https://gitlab.com/fscarmen/warp](https://gitlab.com/fscarmen/warp)）

**- direct(all)**  其它走直连

