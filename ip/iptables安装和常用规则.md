# iptables 安装和常用规则

一、安装iptables

1.1、查看是否安装成功

```
systemctl status iptables
```

1.2、安装iptables

```
yum install iptables-services
或
yum install iptables iptables-services -y
```

1.3、检查是否安装成功

```
systemctl status iptables
```

# 二、iptables 常用规则

## 1、iptables的规则是从上往下的依次执行

只要匹配上，就不再往下匹配

```bash
-A 从上往下插入命令，这个是写先允许，后拒绝的命令
-I 从下往上插入命令，这个是写先拒绝，后允许的命令
```

## 2、限制特定IP端口访问

这里使用从下往上插入命令，这个是写先拒绝，后允许的命令

**栗子1：**

先限制所有IP访问80端口

```
iptables -I INPUT -p tcp --dport 80 -j DROP
```

再允许单个IP访问，要允许多个IP则添加多条即可

```
iptables -I INPUT -s 69.165.74.244 -p tcp --dport 80 -j ACCEPT
```

**栗子2：**
只允许192.168.199.202 访问本机的 3306 8080端口

```
iptables -I INPUT -p tcp -m multiport --dport 3306,8080  -j DROP
iptables -I INPUT -s 192.168.199.202/32 -p tcp -m multiport --dport 3306,8080 -j ACCEPT
```

**栗子3：**
范围端口写法如下：

```
iptables -I INPUT -s 192.168.199.202/32 -p tcp --dport 1000:2000 -j ACCEPT
iptables -A INPUT -s 192.168.199.202/32 -p tcp -m multiport --dport 21,1000:2000 -j ACCEPT
```

**栗子4：**
范围IP写法如下

```
iptables -I INPUT -m iprange --src-range 192.168.199.1-192.168.199.200 -p tcp --dport 2181 -j ACCEPT
```

