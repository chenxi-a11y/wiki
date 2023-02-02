# X-ui面板配置分流规则，实现一个节点访问不同网站按需分流到不同的IP

**概述**

在我们创建节点的时候，我们有时候需要把部分网站的访问IP分配到不同的路由。在X-ui面板上，我们是可以直接操作的。这篇教程就来和大家一起聊聊如何在X-ui设置分流规则，实现一个节点访问不同网站按需分流到不同的IP。

**准备工作**

一台VPS、并已经安装X-ui和设置节点

**配置模板**

面板设置→xray相关设置

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
 "inbounds": [{
 "listen": "127.0.0.1",
 "port": 62789,
 "protocol": "dokodemo-door",
 "settings": {
 "address": "127.0.0.1"
 },
 "tag": "api"
 }],
 "outbounds": [{
 "tag": "IP4-out",
 "protocol": "freedom",
 "settings": {}
 },
 {
 "tag": "IP6-out",
 "protocol": "freedom",
 "settings": {
 "domainStrategy": "UseIPv6"
 }
 }
 ],
 "policy": {
 "system": {
 "statsInboundDownlink": true,
 "statsInboundUplink": true
 }
 },
 "routing": {
 "rules": [{
 "type": "field",
 "outboundTag": "IP6-out",
 "domain": ["ipget.net"] //自定义域名走IPv6出口，例：["geosite:netflix","geosite:*****"]或["netflix.com","****.**"]
 },
 {
 "type": "field",
 "outboundTag": "IP4-out",
 "network": "udp,tcp" //除上述规则外，其他连接走IPv4出口
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

