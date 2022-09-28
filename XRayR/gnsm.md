## 限速功能说明

1. 节点限速：请在SSpanel的节点限速处填写，单位Mbps。
2. 用户限速：请在SSpanel的用户设置处填写，单位Mbps。
3. 限速值设为0，则为不限速。

**本地节点限速设置**

针对不支持远程设置限速的面板：如V2board，可以在本地配置文件`SpeedLimit`设置限速。注意此设置会覆盖远程获取的节点级别限速。



!> 节点限速：所有连接到该节点的用户限速值都会采用`SpeedLimit`中的设置值**（不是端口限速）**

<br><br>

## 设备连接限制功能说明

由于大量面板不再支持远程设备限制指定，现增加本地设备限制参数。

如需启用，可在配置文件中将`DeviceLimit`设为非0值，注意此设置会覆盖远程获取的用户设备限制数目。

配置文件详见：[配置文件说明](https://xrayr-project.github.io/XrayR-doc/xrayr-pei-zhi-wen-jian-shuo-ming/config.html#mian-ban-dui-jie-pei-zhi)

**全局设备限制**

当XrayR版本>=v0.7.1，SSpanel版本>=[2021.9](https://github.com/Anankke/SSPanel-Uim/releases/tag/2021.9)，XrayR将会针对SSpanel启用全局设备限制功能。此时，不同后端结点将会全局限制独立IP连接数量，而非各后端本地限制。

当设备限制为1时，不同结点之间的切换会受到限制，建议至少设置设备数为2。并且由于SSPanel面板限制，IP连接信息可能需要至少2分钟才能传递到全部的后端结点，因此在2分钟内的同时连接将不能被限制。

<br><br>

## 自定义DNS说明

XrayR支持为不同节点设置不同的DNS策略，具体方法如下：

1. 编写dns.json文件，此配置与Xray DNS配置完全相同，请查看：https://xtls.github.io/config/dns.html 获取帮助。
2. 在`config.yml`中配置`DnsConfigPath`为dns.json的路径。
3. 在所需要启用自定义DNS的节点中，将`EnableDNS`设为true。如设为false或者不填则是使用本机DNS。
4. 如果要启用geoip相关配置，请确保`geoip.dat`和`geosite.dat`处于和`config.yml`同一目录。

**DNS解锁样例配置**

```javascript
{
    "servers": [
      "8.8.8.8", 
      {
        "address": "1.1.2.2", // 购买的 DNS 解锁提供的 IP
        "port": 53,
        "domains": [
          "geosite:netflix" 
        ]
      }
    ]
  }
```

**设置IPV6优先**

1. 请先确保主机有ipv6地址，如无，请考虑使用[warp](https://github.com/P3TERX/warp.sh)获取ipv6。
2. 在所需要设置IPV6优先的节点中，将`EnableDNS`设为true。
3. 在所需要设置IPV6优先的节点中，将`SendIP`设为`"::"`。
4. 在所需要设置IPV6优先的节点中，将`DNSType`设为`UseIP`。

至此，XrayR将会优先使用目标网站的ipv6地址进行访问，不会影响默认ipv4站点的访问。~~可以用于解锁Netflix等需求~~

**设置IPV4优先**

1. 在所需要设置IPV4优先的节点中，将`EnableDNS`设为true。
2. 在所需要设置IPV4优先的节点中，将`SendIP`设为`"0.0.0.0"`。
3. 在所需要设置IPV4优先的节点中，将`DNSType`设为`UseIP`。

<br><br>

## 自定义路由功能说明

XrayR完整支持全部的Xray-core所提供的自定义路由功能，具体启用方式如下：

1. 编写 route.json文件，此配置与Xray 路由配置完全相同，请查看：https://xtls.github.io/config/routing.html获取帮助。
2. 在`config.yml`中配置`RouteConfigPath`为route.json的路径。
3. 如果要启用geoip相关配置，请确保`geoip.dat`和`geosite.dat`处于和`config.yml`同一目录。



远程获取的节点自动生成的inboundTag/outboundTag遵循：`NodeType_ListenIP_Port`的形式。如：`V2ray_0.0.0.0_80`。入/出站tag相同。



**自定义路由功能示例**

```text
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
            "outboundTag": "IPv6_out",
            "domain": [
                "geosite:netflix"
            ]
        }
    ]
}
```

<br><br>

## 自定义入口功能说明

XrayR完整支持全部的Xray-core所提供的自定义入口功能，具体启用方式如下：

1. 编写 custom_inbound.json文件，此配置与Xray 出口配置完全相同，请查看：https://xtls.github.io/config/inbound.html获取帮助。
2. 在`config.yml`中配置`InboundConfigPath`为custom_inbound.json的路径。

**自定义入口功能示例**

```text
[
    {
        "listen": "0.0.0.0",
        "port": 1234,
        "protocol": "socks",
        "settings": {
            "auth": "noauth",
            "accounts": [
                {
                    "user": "my-username",
                    "pass": "my-password"
                }
            ],
            "udp": false,
            "ip": "127.0.0.1",
            "userLevel": 0
        }
    }
]
```

<br><br>

## 自定义出口功能说明

XrayR完整支持全部的Xray-core所提供的自定义出口功能，具体启用方式如下：

1. 编写 custom_outbound.json文件，此配置与Xray 出口配置完全相同，请查看：https://xtls.github.io/config/outbound.html获取帮助。
2. 在`config.yml`中配置`OutboundConfigPath`为custom_outbound.json的路径。

**自定义出口功能示例**

```text
[
    {
        "tag": "IPv4_out",
        "protocol": "freedom"
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

<br><br>

## 审计功能说明

1. 请在前端审计规则处填写任意正则表达式，如 `baidu.com`将屏蔽所有baidu的域名，`(.+\.|^)(360|so)\.(cn|com)`将屏蔽360相关网站。
2. 支持输入ip地址屏蔽ip，如`127.0.0.1`。
3. BT协议屏蔽请查看：[自定义路由功能说明](https://xrayr-project.github.io/XrayR-doc/gong-neng-shuo-ming/zi-ding-yi-lu-you-gong-neng-shuo-ming.html)

**本地审计规则设置**

针对不支持远程设置审计规则的面板：如V2board，可以在本地配置文件`RuleListPath`设置本地规则文件路径。规则文件不需要定义文件类型，每条**正则规则**一行，默认本地规则ID标号为-1。

配置文件详见：[配置文件说明](https://xrayr-project.github.io/XrayR-doc/xrayr-pei-zhi-wen-jian-shuo-ming/config.html#mian-ban-dui-jie-pei-zhi)

**本地规则文件示例**

请保证每行只是一个单纯的正则规则，不要包含任何其无关他字符串。

```text
(.+\.|^)(360|so)\.(cn|com)
baidu.com
google.com
127.0.0.1
```

<br><br>

## 自动申请证书说明

XrayR 支持多种自动申请证书配置。申请到的证书将会放在**配置文件(config.yml)目录的`cert`文件夹下**。

以下是自动申请证书的相关配置文件说明。

```yaml
CertConfig:
    CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
    CertDomain: "node2.test.com" # Domain to cert
    CertFile: /etc/XrayR/cert/node2.test.com.cert # Provided if the CertMode is file
    KeyFile: /etc/XrayR/cert/node2.test.com.key
    Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
    Email: test@me.com
    DNSEnv: # DNS ENV option used by DNS provider
        ALICLOUD_ACCESS_KEY: aaa
        ALICLOUD_SECRET_KEY: bbb
```

| 参数         | 选项                       | 说明                                                         |
| :----------- | :------------------------- | :----------------------------------------------------------- |
| `CertMode`   | `none`,`file`,`http`,`dns` | 获取证书的方式。`file`:手动提供，并制定路径。`http`：通过http申请，需要80端口。`dns`：使用dns模式申请，需要制定相关dns服务商配置。`none`：强制关闭tls设置，交由nginx或者caddy处理。 |
| `CertDomain` | 无                         | 申请证书域名                                                 |
| `CertFile`   | 无                         | 手动指定的证书路径                                           |
| `KeyFile`    | 无                         | 手动指定的私钥路径                                           |
| `Provider`   | 无                         | dns提供商，所有支持的dns提供商请在此获取：https://go-acme.github.io/lego/dns/ |
| `DNSEnv`     | 无                         | 采用DNS申请证书需要的环境变量，请参考上文链接内，自己的dns提供商所需要的参数，填写于此。请注意一行一个，填写时需符合yaml文件格式。 |

<br><br>

## Fallback 功能说明

> fallback 为 Xray 提供了高强度的防主动探测性, 并且具有独创的首包回落机制.
>
> fallback 也可以将不同类型的流量根据 path 进行分流, 从而实现一个端口, 多种服务共享.
>
> 目前您可以在使用 VLESS 或者 trojan 协议时, 通过配置 fallbacks 来使用回落这一特性, 并且创造出非常丰富的组合玩法.
>
> ---https://xtls.github.io/config/features/fallback.html

**启用Fallback功能**

设置`EnableFallback`为`true`，并配置`FallBackConfigs`

```yaml
ControllerConfig:
  EnableFallback: true # Only support for Trojan and Vless
  FallBackConfigs:  # Support multiple fallbacks
    -
      SNI: # TLS SNI(Server Name Indication), Empty for any
      Alpn: # Alpn, Empty for any
      Path: # HTTP PATH, Empty for any
      Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
      ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
```

**配置Fallback**

XrayR遵循Xray设计思路，支持一个节点多个Fallback设置，因此`FallBackConfigs`为一个数组，每个子元素示例如下：

```yaml
-
  SNI: # TLS SNI(Server Name Indication), Empty for any
  Alpn: # Alpn, Empty for any
  Path: # HTTP PATH, Empty for any
  Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
  ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
```

**SNI: string**

尝试匹配 TLS SNI(Server Name Indication)，空为任意，默认为 ""

**Alpn: string**

尝试匹配 TLS ALPN 协商结果，空为任意，默认为 ""

有需要时，VLESS 才会尝试读取 TLS ALPN 协商结果，若成功，输出 info `realAlpn =` 到日志。 用途：解决了 Nginx 的 h2c 服务不能同时兼容 http/1.1 的问题，Nginx 需要写两行 listen，分别用于 1.1 和 h2c。 注意：fallbacks alpn 存在 `"h2"` 时，[Inbound TLS](https://xrayr-project.github.io/XrayR-doc/transport.md#tlsobject) 需设置 `"alpn":["h2","http/1.1"]`，以支持 h2 访问。



Fallback 内设置的 `alpn` 是匹配实际协商出的 ALPN，而 Inbound TLS 设置的 `alpn` 是握手时可选的 ALPN 列表，两者含义不同。



**Path: string**

尝试匹配首包 HTTP PATH，空为任意，默认为空，非空则必须以 "/" 开头，不支持 h2c。

智能：有需要时，VLESS 才会尝试看一眼 PATH（不超过 55 个字节；最快算法，并不完整解析 HTTP），若成功，输出 info realPath = 到日志。 用途：分流其它 inbound 的 WebSocket 流量或 HTTP 伪装流量，没有多余处理、纯粹转发流量，实测比 Nginx 反代更强。

注意：fallbacks 所在入站本身必须是 TCP+TLS，这是分流至其它 WS 入站用的，被分流的入站则无需配置 TLS。

**Dest: string|number**

决定 TLS 解密后 TCP 流量的去向，目前支持两类地址：（该项必填，否则无法启动）

1. TCP，格式为 "addr:port"，其中 addr 支持 IPv4、域名、IPv6，若填写域名，也将直接发起 TCP 连接（而不走内置的 DNS）。

2. Unix domain socket，格式为绝对路径，形如 "/dev/shm/domain.socket"，可在开头加 "@" 代表 abstract，"@@" 则代表带 padding 的 abstract。

   若只填 port，数字或字符串均可，形如 80、"80"，通常指向一个明文 http 服务（addr 会被补为 "127.0.0.1"）。

**ProxyProtocolVer: number**

发送 PROXY protocol，专用于传递请求的真实来源 IP 和端口，填版本 1 或 2，默认为 0，即不发送。若有需要建议填 1。

目前填 1 或 2，功能完全相同，只是结构不同，且前者可打印，后者为二进制。Xray 的 TCP 和 WS 入站均已支持接收 PROXY protocol。

> TIP
>
> 若你正在 配置 Nginx 接收 PROXY protocol，除了设置 proxy_protocol 外，还需设置 set_real_ip_from，否则可能会出问题。

**Fallback 示例**

XrayR设置

```text
EnableFallback: true
FallBackConfigs:  # Support multiple fallbacks
  -
    SNI:
    Alpn:
    Path:
    Dest: 8080
    ProxyProtocolVer: 0
```

Nginx设置

```text
server {  
    listen 8080 http2;
  root /var/www/public; # 改成你自己的路径
  index index.php index.html;
  server_name www.test.com; # 改成你自己的域名

  location / {
    try_files $uri /index.php$is_args$args;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass 127.0.0.1:9000; # unix:/run/php/php-fpm.sock;
  }
}
```

参考

[Xray Fallback](https://xtls.github.io/config/features/fallback.html)

<br><br>

## 内存优化相关

**链接控制优化**

通过自定义`ConnetionConfig`连接释放的[相关配置](https://xrayr-project.github.io/XrayR-doc/xrayr-pei-zhi-wen-jian-shuo-ming/config.html#lian-jie-kong-zhi)，可以一定程度优化内存占用

1. 减少`ConnIdle`有可能可以优化高连接数量时的内存占用，但是会导致用户连接延时变高。
2. 在 HTTP 浏览的场景中，可以将 `UplinkOnly` 和 `DownlinkOnly` 设为 0，以提高连接关闭的效率，减少内存占用。
3. 减少`BufferSize`可以优化内存占用，但是可能会导致CPU占用上升。

<br><br>

## 为什么要引入Shadowsocks - V2Ray-Plugin

**Update on 2021/07/04**

我错怪Trojan了，通过后端禁用TLS，配合Nginx的Stream模块也可以实现，Nginx代理处理Trojan的TLS，达到隐藏TLS握手信息的效果，同时可以fallback到http1.1的站点达到比SS更高的性能水平。

**原文**

很多人觉得有Shadowsocks单端口就够了呀，为啥要引入Shadowsocks - V2Ray-Plugin呢？

首先针对近日来的国际互联网通讯情况，我个人分析认为，在特殊时期，会针对go的TLS握手行为进行匹配，并加以阻断。再加上现有大部分的软件（如V2ray-core,Xray-core）都是以go实现的，并采用go的库进行TLS处理。因此在特殊时期，可以对go的TLS握手行为可以进行识别，从而导致端口精准阻断。所以大部分直接采用go进行tls处理的协议，比如Trojan，在近日遭受了严重阻断。同样，使用Caddy反代进行伪装的行为也遭受了阻断。

虽然针对go的TLS库进行识别的行为有极大的误报率（封杀正常的Caddy反代的网站），但是在特殊时期已经被证实是可能实行的了。因我认为，需要隐藏go的TLS握手行为，从而达到更高的隐蔽性。为此，我认为采用C语言编写的NGINX是目前最好的选择。现有情况也表明：Vmess+ws+tls+nginx在目前存活性最好。

Vmess+ws+tls+nginx虽然已经成功隐藏了go的TLS握手信息，但是Vmess协议由于其本身设计，会产生大量的内存占用。同时其基于时间的验证设计，增加了其使用难度。~~而Trojan暂时又不支持使用其他软件进行TLS处理~~。此时Shadowsocks - V2Ray-Plugin成为了最好的选择。

Shadowsocks - V2Ray-Plugin，首先是基于Shadowsocks的。得益于Shadowsocks协议设计，使得Shadowsocks拥有比Vmess更快的速度和不依赖时间的验证。同时V2Ray-Plugin给予Shadowsocks进行websocket混淆和TLS加密的能力。极大增强了Shadowsocks的安全性，使得流量可以直接在公网传输，不再需要隧道。同时可以把TLS交由NGINX处理，隐藏go的相关特征，防止被阻断端口。

综上所述，为了隐藏特征，我强烈建议采用nginx+ws+tls+everything的做法，在目前情况下，nginx+ws+tls+ss的配置会优于nginx+ws+tls+vmess。同时为了长远考虑，我建议所有的协议实现软件采用C语言提供的TLS库进行TLS相关处理，或者参考Shadowsocks分离出插件层，方便使用第三方软件如nginx进行TLS处理。

<br><br>

## Nginx+Trojan暂时滴神！

使用Nginx处理Trojan的TLS，Trojan进行回落。我愿称ta暂时滴神！

**Nginx安装**

CentOS：

```text
 yum update
 yum install -y nginx
 yum install nginx-mod-stream
```

Ubuntu/Debian:

```text
 apt update
 apt install nginx
```

**Nginx配置**

修改/etc/nginx/nginx.conf配置文件：

```text
stream {
    server {
        listen              443 ssl;                    # 设置监听端口为443

        ssl_protocols       TLSv1.2 TLSv1.3;      # 设置使用的SSL协议版本

        ssl_certificate /etc/nginx/ssl/xx.com.pem; # 证书地址
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # 秘钥地址
        ssl_session_cache   shared:SSL:10m;             # SSL TCP会话缓存设置共享内存区域名为
                                                        # SSL，区域大小为10MB
        ssl_session_timeout 10m;                        # SSL TCP会话缓存超时时间为10分钟
        proxy_protocol    on; # 开启proxy_protocol获取真实ip
        proxy_pass        127.0.0.1:1234; # 后端Trojan监听端口
    }
}
```

请将上方代码添加到**http**与**events**中间一行

**/etc/nginx/nginx.conf配置文件参考：**

```text
events {
    worker_connections 768;
    # multi_accept on;
}

stream {
    server {
        listen              443 ssl;                    # 设置监听端口为443

        ssl_protocols       TLSv1.2 TLSv1.3;      # 设置使用的SSL协议版本

        ssl_certificate /etc/nginx/ssl/xx.com.pem; # 证书地址
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # 秘钥地址
        ssl_session_cache   shared:SSL:10m;             # SSL TCP会话缓存设置共享内存区域名为
                                                        # SSL，区域大小为10MB
        ssl_session_timeout 10m;                        # SSL TCP会话缓存超时时间为10分钟
        proxy_protocol    on; # 开启proxy_protocol获取真实ip
        proxy_pass        127.0.0.1:1234; # 后端Trojan监听端口
    }
}

http {

    ##
    # Basic Settings
    ##
```

**注意事项：**

**1. 请配置SSL证书**

**2. proxy_pass 127.0.0.1:1234 后端Trojan监听端口与您网站前端节点监听端口一致**

**3. listen端口可以1-65535随意修改，此处为客户端连接端口**



centos系统请关闭selinux，不然可能导致转发失败。

sudo setenforce 0

sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config



**XrayR Trojan配置**

**关键配置：**

```text
ListenIP: 127.0.0.1
EnableProxyProtocol: true
EnableFallback: true
CertMode: none
```



注意1：请务必确保CertMode为none，交由Nginx处理tls





注意2：在回落时请确保回落站点是http1.1，nginx如果有一个站点是h2会导致全部站点都变成h2（巨坑）



**完整样例**

```text
  -
    PanelType: "SSpanel" # Panel type: SSpanel, V2board, PMpanel
    ApiConfig:
      ApiHost: "https://xxx.com"
      ApiKey: "123"
      NodeID: 1
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 10 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/XrayR/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 127.0.0.1 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      EnableProxyProtocol: true # Only works for WebSocket and TCP
      EnableFallback: true # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: fake.website.com:80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: none # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/XrayR/cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: /etc/XrayR/cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```

**重启并检查 Nginx 和 XrayR**

```text
systemctl restart nginx
XrayR restart
systemctl status nginx
XrayR status
```