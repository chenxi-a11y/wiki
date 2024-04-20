# Cloudflare设置流程 免费CDN加速你的网站【2024年最新】

## **什么是Cloudflare**

Cloudflare 是一种内容分发网络服务

CDN 是一种优化网站请求处理的机制。它是在用户访问网站 (服务器) 时用户与网站服务器之间的中间层。

CDN 可以将浏览器发起的请求信息缓存起来，并具有负载均衡的功能，因此即使突然大量访问，也能维持网站服务而不使服务器崩溃。此外，Cloudflare 的标准功能中还配备了优化缓存数据的功能。

CDN 产品有 Akamai 和 CloudFront 等，而 Cloudflare 也是其中之一。
其中，Cloudflare CDN 的全球市场份额排名第一，被超过 80% 的用户选择使用。

![img](./tp/Cloudflare设置流程.tp/Cloudflare-有无CDN的区别.webp)

Cloudflare优势：

- **DDoS保护**：过滤恶意流量，保护网站免受分布式拒绝服务的攻击。
- **内容分发网络（CDN）**：帮助加速网站内容在全球范围内的传输。
- **SSL/TLS加密安全保护**：帮助保护网站和访问者的安全。
- **提升网页访问速度**：通过压缩代码、压缩资源和缓存内容来优化网站性能。
- **详细流量分析和报告**：包括请求、带宽使用和安全事件等信息。
- **简单直观的界面**：易于使用的管理网站设置和配置。
- **免费和收费计划**：可以是获得CDN、DDoS保护和安全等优势的经济实惠方式。

## **注册Cloudflare 账号，添加域名、修改DNS并激活邮箱**

[官网 Cloudflare 注册登录界面](https://dash.cloudflare.com/sign-up)

开始使用Cloudflare

![img](./tp/Cloudflare设置流程.tp/Cloudflare-开始使用Cloudflare.webp)

输入你的邮箱和密码创建账户

选择加速和保护您的网站或应用程序

![img](./tp/Cloudflare设置流程.tp/Cloudflare-选择加速和保护您的网站或应用程序.webp)

添加你的站点域名地址

![img](./tp/Cloudflare设置流程.tp/Cloudflare-添加你的站点域名地址.webp)

这里我添加本站点网站域名地址：for-tiger.com
注意：不要填带有www.域名。

普通用户直接选择Free计划

![img](./tp/Cloudflare设置流程.tp/Cloudflare-普通用户直接选择Free计划.webp)

Cloudflare 会自动扫描你的域名DNS记录

进入设置域名解析详情页面

![img](./tp/Cloudflare设置流程.tp/Cloudflare-扫描你的域名DNS记录.webp)

更改你的名称服务器

![img](./tp/Cloudflare设置流程.tp/Cloudflare-更改你的名称服务器-1.webp)

![img](https://for-tiger.com/wp-content/uploads/2023/01/Cloudflare-%E6%9B%B4%E6%94%B9%E4%BD%A0%E7%9A%84%E5%90%8D%E7%A7%B0%E6%9C%8D%E5%8A%A1%E5%99%A8-2.webp)

替换域名的DNS地址

修改域名DNS服务器

![img](./tp/Cloudflare设置流程.tp/Cloudflare-修改域名DNS服务器-阿里云服务器.webp)

这里我们用阿里云服务器修改为例：
阿里云的**域名控制台**→找到对应的域名，选择**管理**→在左边栏找到**DNS修改**
注意：解析DNS需要一段时间，通常 5-10 分钟才会生效。

检查邮件，确保已正常激活

![img](./tp/Cloudflare设置流程.tp/Cloudflare-检查邮件，确保已正常激活-1.webp)

![img](./tp/Cloudflare设置流程.tp/Cloudflare-检查邮件，确保已正常激活-2.webp)

Cloudflare 成功保护您的站点

![img](./tp/Cloudflare设置流程.tp/Cloudflare-成功保护您的站点.webp)

完成~进入快速入门指引

## **快速入门指南设置**

开始使用

![img](./tp/Cloudflare设置流程.tp/Cloudflare-快速入门指南.webp)

自动HTTPS重写

![img](./tp/Cloudflare设置流程.tp/Cloudflare-自动HTTPS重写.webp)

始终使用HTTPS

![img](./tp/Cloudflare设置流程.tp/Cloudflare-始终使用HTTPS.webp)

**Auto Minify**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-Auto-Minify.webp)

**Brotli**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-Brotli.webp)

配置摘要确认

![img](./tp/Cloudflare设置流程.tp/Cloudflare-配置摘要确认.webp)

完成

## **详细设置**

### **SSL/TLS**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-SSLTLS.webp)

### ***\*Cloudflare Tiered Cach\****

![img](./tp/Cloudflare设置流程.tp/Cloudflare-Tiered-Cach.webp)



建议打开，Argo Tiered Cache 无缓存时可以减少回源

这种做法通过限制可以向源服务器请求内容的数据中心的数量来提高带宽效率，可以减少源服务器负载，让网站的运营更具成本效益。

### **Always OnlineTM**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-Always-Online.webp)



为你网页创建副本

如果启用「Always Online」即使你源服务器一时瘫痪，也会显示 Cloudflare 创建好的网站副本。这样就可以始终向访问者提供信息。

### ***\*Rocket Loader\**TM**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-Rocket-Loader.webp)



注意

该选项需要亲测，开启后页面浏览速度提升显著，但是部分用户访问页面可能会出现问题。

### **测试速度Speed**

![img](./tp/Cloudflare设置流程.tp/Cloudflare-速度Speed.webp)

## **防火墙规则**

### **什么是Cloudflare WAF**

**WAF**全称为“Web应用防护系统”，Web Application Firewall

Cloudflare WAF 是 Cloudflare 提供的一种安全功能，能帮助保护网站免受各种恶意攻击，如 SQL 注入、跨站脚本（XSS）等网络应用程序漏洞。它通过规则集和机器学习算法检测和阻止恶意流量。它还允许您基于特定的安全需求创建自定义规则集。除了阻止恶意流量之外，WAF 还可以通过缓存和压缩响应来加速网站内容的传输。它还提供了有关安全事件的详细分析和报告，因此您可以快速识别和应对任何潜在威胁。

### **通过Cloudflare 屏蔽国家IP访问网页**

![img](./tp/Cloudflare设置流程.tp/加油-2.webp)

我不想让某些国家查看我的网页，有没什么办法。



![img](./tp/Cloudflare设置流程.tp/对的-3.webp)

Cloudflare 就能做到，它有很多防火墙选项，

包括禁止部分国家和地方访问网站。





为什么要使用防火墙？

在网络上有大量的垃圾信息，对于WordPress网站来说，只要开启了评论功能，就会不可避免地遇到垃圾评论。即使评论需要人工审核，每天在后台审核几条或几十条垃圾评论也非常繁琐。

面对这种情况，有些网站只能无奈地关闭评论功能。通过使用 Cloudflare 防火墙（免费）来阻止垃圾评论，这种方法更人性化。相比 Akismet 插件，不会消耗额外的服务器性能。

创建防火墙规则

![img](./tp/Cloudflare设置流程.tp/Cloudflare-创建防火墙规则.webp)

添加禁止访问的国家

![img](./tp/Cloudflare设置流程.tp/Cloudflare-添加禁止访问国家.webp)



活用CloudFlare的防火墙功能，提高站点安全性

当然不仅限于屏蔽国家/地区，你还可以选择主机名、ASN、Cookie、州、IP源地址、主机名等，来提高你的网站安全。

确认阻止并部署防火墙规则

![img](./tp/Cloudflare设置流程.tp/Cloudflare-部署防火墙规则.webp)



可以同时添加多个国家

字段最后面有 **And** / **Or** 两个选项，你可以同时添加多个国家，你也可以像我这样单独管理。

※免费用户只有5条规则可以添加，超出后需要付费。

![img](./tp/Cloudflare设置流程.tp/Cloudflare-防火墙开关.webp)

你已禁止该国家访问

![img](./tp/Cloudflare设置流程.tp/Cloudflare-禁止国家访问成功.webp)如何你用日本IP 进行页面范围就会出现以上界面

**完成**