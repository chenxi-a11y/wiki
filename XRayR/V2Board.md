# 基本对接配置

1. 在`config.yml`中配置`PanelType: "V2board"`。
2. V2board只有V2ray节点类型支持面板配置审计规则，其他协议请使用XrayR[本地审计功能](https://xrayr-project.github.io/XrayR-doc/gong-neng-shuo-ming/rule.html)。
3. 启用vless和xtls，请在配置文件中手动启动，V2board不支持在线配置，同时V2board不支持vless和xtls下发，请手动修改客户端配置，或者自行寻找其他解决方案。

配置文件详见：[配置文件说明](https://xrayr-project.github.io/XrayR-doc/xrayr-pei-zhi-wen-jian-shuo-ming/config.html)

**对接vmess+ws**

v2board需要在传输协议配置中增加以下内容，配置ws的路径：

```
{
  "path": "/name",
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流。

**对接vmess+ws+tls**

v2board需要在传输协议配置中增加以下内容，配置ws的路径和tls的域名：

```
{
  "path": "/",
  "headers": {
    "Host": "v2ray.com"
  }
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流，`"Host"`后面的域名更改为自己的伪装域名。

**对接vmess+grpc**

为了成功支持clash连接，在对接vmess+grpc时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "serviceName": "name",
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流。

**对接vmess+tcp+http**



原生V2board不支持tcp+http订阅下发，请自行寻找解决方法，或手动配置客户端文件。



在对接vmess+tcp+http时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "header": {
    "type": "http",
    "request": {},
    "response": {}
  }
}
```

其中`request`和`response`中的内容请自行参照[Xray-core文档](https://xtls.github.io/config/transports/tcp.html#httpheaderobject)设置。

**面板对接配置**

```
ApiConfig:
    ApiHost: "你的前端域名"
    ApiKey: "前端的密钥"
    NodeID: 节点ID
    NodeType: V2ray # Node type: V2ray, Trojan, Shadowsocks, Shadowsocks-Plugin
    Timeout: 30 # Timeout for the api request, Default is 5 sec
    EnableVless: false # Enable Vless for V2ray Type
    EnableXTLS: false # Enable XTLS for V2ray and Trojan
    SpeedLimit: 0 # Local settings will replace remote settings, 0 means disable
    DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
    RuleListPath: # /etc/XrayR/rulelist Path to local rulelist file
    DisableCustomConfig: false # Disable custom config
```

**证书申请相关配置**

```
CertConfig:
    CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
    CertDomain: "节点的域名" # Domain to cert
    CertFile: /etc/XrayR/cert/node2.test.com.cert # Provided if the CertMode is file
    KeyFile: /etc/XrayR/cert/node2.test.com.key
    Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
    Email: 注册的邮箱
    DNSEnv: # DNS ENV option used by DNS provider
        ALICLOUD_ACCESS_KEY: 以是注册的邮箱
        ALICLOUD_SECRET_KEY: DNS密钥
```

