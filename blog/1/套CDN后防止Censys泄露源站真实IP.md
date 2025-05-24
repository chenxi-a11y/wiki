# 套CDN后防止Censys泄露源站真实IP

网站即使是套了cloudflare等cdn，通过访问https://ip地址，会暴露域名。

有些无良爬虫可通过HTTPS访问扫描全网IP，暴露证书、同时暴露你的域名。即使你套了CDN也逃不掉。像censys.io和shodan就是提供这种服务的。

示范一下扫描IP命令：

```
curl -v -k https://35.186.1.1
```

```
curl -v -k https://35.186.1.1
* Rebuilt URL to: https://35.186.1.1/
*   Trying 35.186.1.1...
* TCP_NODELAY set
* Connected to 35.186.1.1 (35.186.1.1) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: CN=normal_domain.tld
*  start date: Nov 15 05:41:39 2019 GMT
*  expire date: Nov 14 05:41:39 2020 GMT
*  issuer: CN=normal_domain.tld
*  SSL certificate verify result: unable to get local issuer certificate (20), continuing anyway.
> GET / HTTP/1.1
> Host: 35.186.1.1
> User-Agent: curl/7.58.0
> Accept: */*
> 
* Empty reply from server
* Connection #0 to host 35.186.1.1 left intact
curl: (52) Empty reply from server
```

可以看见，证书和域名都暴露了。

### 解放方法

**1、如果使用的宝塔面板**

随意新添加一个站点，比如blank.com，自签证书（本文末尾会提供空白的自签证书，任何域名都可使用），打开 【站点设置->配置文件】，在如图位置加上`return 444;`

![img](./tp/套CDN后防止Censys泄露源站真实IP.tp/88.png)

最后将该站点，设置为“默认站点”即可。

**2、Nginx配置**

Nginx 版本高于等于 1.19.4，才可以使用 ssl_reject_handshake 特性来防止 SNI 信息泄露。如果 Nginx 版本太低，可以看看姥爷的这篇文章[自编译 Nginx](https://111111.online/posts/nginx_make.html)。

下面就来讲讲 ssl_reject_handshake 怎么用。只要新添加个443端口默认块就可以了。

```
#新添加的443端口块，如果使用了错误的 Hostname，SSL 握手会被拒绝
server {
    listen 443 ssl default_server;
    #如果有IPv6地址需加入下面这行，否则不用下面这行
    listen [::]:443 ssl default_server;
    ssl_reject_handshake on;
}

#常规的443端口，包含正确的域名和证书。对于携带正确 Hostname 的请求，服务器会继续做后续处理
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name example.com;
    ssl_certificate example.com.crt;
    ssl_certificate_key example.com.key;
}
```

重启nginx即可。

**3、CDN IP加白名单**

给CDN IP添加到白名单，只有CDN IP才可以访问源站。如果套的是cloudflare，cloudflare IP为：https://www.cloudflare.com/ips/

**4、屏蔽 censys.io和shodan 的IP段和UA**

这种方法只能防君子不防小人，而且还有很多未知的类似服务的IP和UA。

以censys.io为例，将它的IP段加到nginx deny配置中，如果用的是aapanel或者宝塔，可以在/www/server/panel/vhost/nginx下新建一个blockips.conf配置文件：

```
deny 66.132.159.0/24;
deny 162.142.125.0/24;
deny 167.94.138.0/24;
deny 167.94.145.0/24;
deny 167.94.146.0/24;
deny 167.248.133.0/24;
deny 192.35.168.0/23;
deny 199.45.154.0/24;
deny 199.45.155.0/24;
deny 206.168.34.0/24;
deny 206.168.35.0/24;
```

如果服务器带IPv6，还需添加IPv6段：

```
deny 2620:96:e000:b0cc:e::/64;
deny 2602:80d:1000:b0cc:e::/80;
deny 2602:80d:1003::/112;
deny 2602:80d:1004::/112;
```

censys.io的IP段有时会更新，所以留意下官方的说明：https://support.censys.io/hc/en-us/articles/360043177092-Opt-Out-of-Data-Collection

为了以绝后患，可以将其UA和ASN (censys.io的ASN为398324，shodan的ASN为10439) 加入到cloudflare防火墙。

最后附上空白自签证书。

privkey.pem

```
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDXyF6m81zOeoOPvfk6nGKtyfczRG6/yeSkcc+66vGvq0s8oB7V
cCzLl1YcNsru3ixelPR2z1zvjKqa9/Aqh8+TvP1kGGbLD/mynjnj8l+0vVzZ+vnz
AH0RN9fpqzlpHmFBHQzQ25AtIAH8pXOL1541YN0TNPRA3kHUCL0FH8CkwwIDAQAB
AoGAQ4ejh6AV5VCWJ8AOZXdXsofIYzUBa+glNAmiNx8b8BwteZWq0KVAf56nBkFn
lQXW4OrA7wXKUfW11rXNZaIHJePJXv1swkN9+Em18Hon6BrtcqnKAwzAbhok3SzY
IVjI/zrgOABH6+ii77xCRBzI1itVPNN88DAUHC7PYLYiaaECQQD7PSoij37+kMc/
wPeEkl9r3vzU0OrsCsjU8Ev714OaoL/SIuAh6nsiRh9rcbUrrpGSSzIcmsk9HMDa
hXBNkNl5AkEA298yQvssaUc4tbEWxAVfd9DsHJdCdbXfgf9Dy5/tpCzYncY7T0du
VVHqKu3jXWoMc5XlesiCOerU/DIlMM8dGwJBANQn7GLO5iC1xWvS2bF7oVSIMtzL
pvW4jaszWBbNAPccc59RkA9T4LMqn/GtTZ4bhhYRpbl+BB21IC3nrNPzU5ECQG8T
Ln0QDruQs2F2eR3F6RjKfr1i3LxCiQtPPZycypzp2vS5tDS0zVRk8XuGehoy/N9X
lnqU2NURgU92tbsWpokCQQDdc9tU3B/OM/YfzUNwvOLmUVwrJX6PFSFsOn+XHrCC
q9LcGEAHyzaf5GEWje84ee4rkv5oaZcwll3dg4IioBnC
-----END RSA PRIVATE KEY-----
```

fullchain.pem

```
-----BEGIN CERTIFICATE-----
MIIBkjCB/AIJAI3bCYqa39hiMA0GCSqGSIb3DQEBBQUAMA0xCzAJBgNVBAYTAiAg
MCAXDTE4MTEyNDA5MDMzOFoYDzIwOTkxMjMxMDkwMzM4WjANMQswCQYDVQQGEwIg
IDCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA18hepvNcznqDj735Opxircn3
M0Ruv8nkpHHPuurxr6tLPKAe1XAsy5dWHDbK7t4sXpT0ds9c74yqmvfwKofPk7z9
ZBhmyw/5sp454/JftL1c2fr58wB9ETfX6as5aR5hQR0M0NuQLSAB/KVzi9eeNWDd
EzT0QN5B1Ai9BR/ApMMCAwEAATANBgkqhkiG9w0BAQUFAAOBgQBiqHZsuVP09ubT
GzBSlAFEoqbM63sU51nwQpzkVObgGm9v9nnxS8Atid4be0THsz8nVjWcDym3Tydp
lznrhoSrHyqAAlK3/WSMwyuPnDCNM5g1RdsV40TjZXk9/md8xWxGJ6n1MoBdlK8T
H6h2ROkf59bb096TttB8lxXiT0uiDQ==
-----END CERTIFICATE-----
```

本文参考：

https://1024.day/d/439

https://1024.day/d/690

https://blog.chrxw.com/archives/2019/10/16/393.html

https://hostloc.com/thread-546327-1-1.html

https://search.censys.io/

https://www.shodan.io/