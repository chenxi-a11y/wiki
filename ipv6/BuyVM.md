# BuyVM 

**Ubuntu 20.04 增加 IPv6 地址**

本文以 BuyVM 装有 Ubuntu 20.04.3 的 VPS 为例，希望能够避免更多人踩坑甚至遭遇服务器失联。

尤其对于并不是 BuyVM 的 VPS 用户来说，情况不一定相同，建议同时参照下面加粗的参考文献研究。

**分配 IPv6 地址**

BuyVM 的 VPS 本身不带 IPv6，仅有一个 IPv4。

要分配一个 IPv6 地址，在 Stallion 管理界面 - Virtual Services-Networking-IPv6 内点击 Assign IPv6 Address。

你可以随机选择一个，也可以在范围内指定一个 IP 地址。点击 Add IPv6 Address，就可以了。

暂时不要关闭 Stallion，等待下一步操作。

**编辑 Netplan 设置**

网络上搜到的教程大多是旧版 Ubuntu 所使用的方法，而 Ubuntu 20.04.3 已经开始使用 Netplan。

使用 SSH 连接到服务器。如果你担心因操作不当而失联，可以直接使用 VNC 连接，方法见下章。

Netplan 的默认配置文件处于 / etc/netplan 中，里面应该有一个 YAML 文件，即其配置文件。

使用你喜欢的代码编辑器（Vim，Nano，whatever）打开此文件，你将看到如下形式的配置文件。有些许差异可以忽略。

**YAML**

```
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
```

将其改成如下内容，其中核心内容将在下面说明。

**YAML**

```
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
      dhcp6: true
      gateway4: x.x.x.x # IPv4 网关
      gateway6: xx::1 # IPv6 网关
      addresses: [x.x.x.x/24,'x:x::/Bitmask'] # 分别为 IPv4 地址、IPv6 地址、IPv6 Bitmask
```

从 Stallion 刚刚的页面中点击 IPv4 选项卡，然后在下方的 IPv4 addresses 中的 Normal 项找到设置图标。点击它，再点击 Network Settings。

这个 IP Address 就是你的 IPv4 地址，Gateway 就是 IPv4 网关，分别将其替换入上面的修改内容。如果你的 Netmask 也是 255.255.255.0，那么 / 24 就不需要变动，它们的意义相同，只是一个 Netmask 和 Bitmask 的相互转换，前者是 IPv4 惯用表述，后者则是 IPv6 的表述。Netplan 统一用后者。

另外，对于某几个地点的 Gateway，[Frantech 官网](https://wiki.buyvm.net/doku.php/kvm#networking) 也有说明。

同理，进入刚刚你分配的 IPv6 地址设置中，替换 IP Address 和 Gateway 为对应的值。Bitmask 的值则替换为网页上 Netmask/Bitmask 的值，一般为 48。

保存配置文件，使用命令 

```
sudo netplan try
```

 可以自动检查配置文件，如果看起来没有问题的话就可以按回车继续了。然后使用 `

```
sudo netplan apply
```

 来应用更改。

**检查网络连接**

一小段时间之后，使用 `

```
networkctl status eth0
```

` 命令查看 eth0 端口情况。除了查看 Address 信息有没有错误之外，最重要的是 State。如果是绿色的 routable(configured)，那么一切正常。否则，degraded 表示可能没有连接公网，而若下方 log 中提示 No Route to Host 则可能代表 Gateway 设置错误。

这之后，可以使用 `

```
ping6 google.com
```

` 测试一下 IPv6 下的网络连接。也可以用其他设备 Ping 你刚刚分配的 IPv6 地址。如果都不会提示 Network Unreachable，那么就万事大吉了。

**如果你的服务器已经因此失联**

你永远可以相信 VNC。在 Stallion 右上方的 Console 内点击 Direct VNC Connection，然后在你的 VNC 客户端（官方推荐的 TightVNC 就不错）Viewer 上输入网页中弹出的用户名和密码，就可以连接到服务器了。

**参考文献**

- https://hostloc.com/thread-745390-1-1.html
- https://oldtang.com/2314.html
- https://developer.aliyun.com/article/744737
- https://askubuntu.com/questions/328265/trying-to-enable-ipv6-results-in-a-no-route-to-host-error
- [**https://zhuanlan.zhihu.com/p/46544606**](https://zhuanlan.zhihu.com/p/46544606)