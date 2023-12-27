# 使用iptables屏蔽国外ip

## 一、安装iptables

安装iptables

```cobol
yum install iptables-services
```

1.3、检查是否安装成功

```cobol
systemctl status iptables
```

## 二、先限制所有IP访问80 443端口

先限制所有IP访问80 443端口

```
iptables -I INPUT -p tcp -m multiport --dport 80,443  -j DROP
iptables -I INPUT -p udp -m multiport --dport 80,443  -j DROP
```

再添加单个IP白名单，要允许多个IP则添加多条即可

```
iptables -I INPUT -s 179.61.251.219 -p tcp -j ACCEPT
```

