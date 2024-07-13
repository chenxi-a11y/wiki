查看 vps 进程网络流量

弄好了 vps 以后，感觉网络流量走的有点多，决定查查看到底什么情况。
首先安装 sar 来看看各个设备消耗的流量

```code
apt-get install sysstat
```

sar 的参数 DEV 表示网口， 1 表示每秒去一次数值，4表示连续取值4次。

```code
sar -n DEV 1 4
```

然后看到 eth0 消耗的流量比较大，每秒 200k+。

再安装 iptraf 来具体查查各个端口的数据量。

```code
apt-get install iptraf
```

然后使用 

```
iptraf-ng
```

 来查看具体端口的数据量，我这边发现是 22号和 30512号特别多。
然后使用 netstat 来看看 具体的进程。

```code
netstat -tunp | grep 22
```

也可以使用 lsof -i:22 来查看端口的进程。

还可以安装 iftop, 使用 iftop -P 来查看具体流量。