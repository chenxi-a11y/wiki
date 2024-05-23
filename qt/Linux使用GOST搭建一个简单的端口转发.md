# Linux 使用GOST搭建一个简单的端口转发

**介绍**

Gost是一个功能多样且实用的安全隧道工具，使用的是go语言编写 

GitHub项目：https://github.com/ginuerzh/gost 

Gost文档：https://docs.ginuerzh.xyz/gost 

Gost搭建Socks5：https://sunpma.com/862.html



**特性**

- Tunnel UDP over TCP
- TCP/UDP透明代理
- 本地/远程TCP/UDP端口转发
- 支持Shadowsocks(TCP/UDP)协议
- 支持SNI代理
- 权限控制
- 负载均衡
- 路由控制
- DNS解析和代理
- TUN/TAP设备

- 多端口监听
- 可设置转发代理，支持多级转发(代理链)
- 支持标准HTTP/HTTPS/HTTP2/SOCKS4(A)/SOCKS5代理协议
- Web代理支持探测防御
- 支持多种隧道类型
- SOCKS5代理支持TLS协商加密



**Linux安装**

**ARM**（比如甲骨文ARM机型）

```
 wget "https://github.com/ginuerzh/gost/releases/download/v2.8.1/gost_2.8.1_linux_arm.tar.gz"
 
 tar -zxvf gost_2.8.1_linux_arm.tar.gz
 
 mv gost_2.8.1_linux_arm/gost /usr/bin/gost
 
 chmod +x /usr/bin/gost
```



**X64**

```
 wget "https://github.com/ginuerzh/gost/releases/download/v2.8.1/gost_2.8.1_linux_amd64.tar.gz"
 
 tar -zxvf gost_2.8.1_linux_amd64.tar.gz
 
 mv gost_2.8.1_linux_amd64/gost /usr/bin/gost
 
 chmod +x /usr/bin/gost
```



**TCP 转发**

```
 gost -L=tcp://:本地使用端口/远程服务IP:远程服务端口
```



**UDP 转发**

```
 gost -L=udp://:本地使用端口/远程服务IP:远程服务端口
```



**全协议转发（TCP+UDP）**

```
 gost -L=:本地使用端口/远程服务IP:远程服务端口
```



**小飞机转发**

```
 gost -L=:本地使用端口 -F=ss://加密方式:密码@远程服务IP:远程服务端口?nodelay=true
```

示例：使用A服务器的 **8888** 端口转发IP为 **114.114.114.114** 的B服务器的 **9999** 端口（TCP+UDP） **gost -L=:8888/114.114.114.114:9999** 创建命令后我们就可以连接到A服务器的 **8888** 端口，从而使用B服务器的 **9999** 端口； 如果服务器使用了宝塔面板需要在面板安全设置中放行对应的端口；



如果测试后没有问题，就可以使用 **nohup** 命令将转发设置挂载到后台持续运行； **nohup** 命令重启后会失效，开机后再次挂载即可重新使用；

- 挂载后台

```
 nohup gost -L=:本地使用端口/远程服务IP:远程服务端口 > /dev/null 2>&1 &
```

- 关闭挂载

```
 kill -9 $(ps aux | grep "gost" | sed '/grep/d' | awk '{print $2}')
```