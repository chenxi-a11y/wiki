**配置思路：先确认debian是否开启了ipv6然后再去配置静态IP， 不然就算配置了ipv6也不会生效**

### 1.检查系统是否开启ipv6

```
cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```

如果输出 0 ，则表示启用了，如果输入 1 则表示没有启用

若输出为1则需编辑配置文件 /etc/sysctl.conf 若输出为0则忽略以下内容

```
vim /etc/sysctl.conf
找到 net.ipv6.conf.all.disable\_ipv6 = 1，将1改为0即可。

一直向下滚动，并在末尾添加以下行：

net.ipv6.conf.all.autoconf = 0
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.eth0.autoconf = 0
net.ipv6.conf.eth0.accept_ra = 0
要检查运行：

sysctl -p
然后尝试重启网络

systemctl restart networking
```

重启网络和系统

```
systemctl restart networking.service

reboot
```



### 2.添加 IPv6 地址

```
编辑网卡文件
vi /etc/network/interfaces

二、添加 IPv6 地址
iface eth0 inet6 static
address YOUR_PUBLIC_IPV6_ADDRESS
netmask 64
gateway YOUR_PUBLIC_PIV6_GATEWAY
autoconf 0
dns-nameservers 2001:4860:4860::8844 2001:4860:4860::8888 209.244.0.3

将eth0设置为ipv6静态模式 iface eth0 inet6 static static表示使用固定ip，dhcp表述使用动态ip
将IP设置为fe80:66:666:666::1FA2 address fe80:66:666:666::1FA2
将掩码设置为64位 netmask 64
将网关设置为fe80:66:666:666::1 gateway fe80:66:666:666::1

三、重启网络
/etc/init.d/networking restart

重启服务器
reboot

四、测试 IPv6 链接
ping6 ipv6.google.com
ping6 google.com
```

