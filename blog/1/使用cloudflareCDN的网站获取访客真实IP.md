# 使用cloudflare CDN的网站获取访客真实IP

之前有一篇文章是介绍[宝塔获取cloudflare CDN访客真实IP](https://www.168itw.com/web-server/nginx-cdn-real-ip/)的文章，但是那篇文章比较粗糙。下面结合cloudflare官方的说明文档，做一些补充说明。

### 一、CF-Connecting-IP 和 X-Forwarded-For

cloudflare将原始访问者 IP 地址显示在名为 CF-Connecting-IP 和 X-Forwarded-For 的附加 HTTP 标头中。所以你可以选择用：

```
real_ip_header CF-Connecting-IP; 
```

或者：

```
real_ip_header X-Forwarded-For;
real_ip_recursive on;
```

来获取访客真实IP。

### 二、ngx_http_realip_module模块

nginx服务器获取realip是依赖ngx_http_realip_module模块的，所以如果你用的是nginx，必须要编译ngx_http_realip_module模块。使用nginx -V命令查看nginx是否已经编译该模块，没有的话需要自己编译。

其它服务器参考[cloudflare官方说明文档](https://developers.cloudflare.com/support/other-languages/简体中文/恢复原始访问者-ip/)。

### 三、set_real_ip_from的使用

如果是 set_real_ip_from 0.0.0.0/0; 则表示信任所有前代理传递的真实用户ip，会有安全隐患，我们可以设置为只信任cloudflare的IP：

```
server
{
listen 443 ssl http2;
listen [::]:80;
server_name 168itw.com;

set_real_ip_from 103.21.244.0/22;
set_real_ip_from 103.22.200.0/22;
set_real_ip_from 103.31.4.0/22;
set_real_ip_from 104.16.0.0/12;
set_real_ip_from 108.162.192.0/18;
set_real_ip_from 131.0.72.0/22;
set_real_ip_from 141.101.64.0/18;
set_real_ip_from 162.158.0.0/15;
set_real_ip_from 172.64.0.0/13;
set_real_ip_from 173.245.48.0/20;
set_real_ip_from 188.114.96.0/20;
set_real_ip_from 190.93.240.0/20;
set_real_ip_from 197.234.240.0/22;
set_real_ip_from 198.41.128.0/17;
set_real_ip_from 2400:cb00::/32;
set_real_ip_from 2606:4700::/32;
set_real_ip_from 2803:f800::/32;
set_real_ip_from 2405:b500::/32;
set_real_ip_from 2405:8100::/32;
set_real_ip_from 2c0f:f248::/32;
set_real_ip_from 2a06:98c0::/29;

#X-Forwarded-For和CF-Connecting-IP二选一
real_ip_header    X-Forwarded-For;
real_ip_recursive on;
#real_ip_header CF-Connecting-IP; 
```

cloudflare IP段：https://www.cloudflare.com/ips/

注意：添加在nginx的http{}中，则全局应用在下面的所有server中，即所有网站生效；添加在网站对应的server{}中，则只有该网站中生效。

特别注意：如果同时运行了docker，且有反代docker服务，则需要将docker内网IP段也要加到set_real_ip_from中。

查看docker容器IP：

```
docker inspect 容器id
```

添加docker内网IP段：

```
set_real_ip_from 172.18.0.0/24
```

### 四、多级反向代理

如果cloudflare之后还有多级反向代理，可以在cloudflare后的第一级反代只允许cf IP回源，后面的允许上一级反代IP回源。

### 五、开启CLOUDFLARE的回源认证

cloudflare后台 SSL/TLS – Overview：SSL/TLS encryption mode设置为Full或者Full (strict)

SSL/TLS – Origin Server 开启：Authenticated Origin Pulls

在对应主机的配置文件的server块内加入下列代码:

```
ssl_client_certificate /etc/nginx/certs/cloudflare.pem;
ssl_verify_client on;
```

cloudflare.pem文件下载地址：https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/set-up/zone-level/



参考文档:

https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/set-up/zone-level/

https://limbopro.com/archives/15342.html

https://sakurabakiyoka.com/post/30.html