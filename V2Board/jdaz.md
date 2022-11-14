# **XrayR**节点安装

## 1. 进入V2Board后台

> 后台地址/admin
> 账号密码在安装期间会要求输入
> 进入节点列表，选择一个协议（这里以v2ray为例）

![image-20221113122611806](tp/jdaz.tp/image-20221113122611806.png)

## 2. 添加节点

- 节点名称、节点标签、权限组 根据自己的需求填
- 倍率指用户使用多少流量，实际扣多少倍流量
- 节点地址建议用域名（解析到节点IP），把tls勾选，端口均填443，若直接使用IP裸奔容易被墙（加入gfw黑名单）
- 协议根据自己需求填，协议配置一定要填（传输协议旁边的编辑配置按钮）

!> 注意：节点地址如果直接用服务器ip的话就选择不支持tls，如果用域名的话就选择支持tls（用域名要先把域名解析到节点IP）

!> 连接端口和服务端口填一样，不一样可能没网，例如要用80端口就两个都填80

![image-20221113122625718](tp/jdaz.tp/image-20221113122625718.png)

websocket传输协议——编辑配置

传输协议为 Websocket，必须要点击<传输协议>旁的<编辑配置>，并填写以下内容，path 可以自定义，以 / 开头

```
{
"path": "/"
}
```

**这里说下基本对接配置**

**对接vmess+ws**

v2board需要在传输协议配置中增加以下内容，配置ws的路径：

```
{
  "path": "/name",
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流。

**对接vmess+ws+tls**

v2board需要在传输协议配置中增加以下内容，配置ws的路径和tls的域名：

```
{
  "path": "/",
  "headers": {
    "Host": "v2ray.com"
  }
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流，`"Host"`后面的域名更改为自己的伪装域名。

**对接vmess+grpc**

为了成功支持clash连接，在对接vmess+grpc时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "serviceName": "name",
}
```

其中`"name"`换成任意字符串，可用于nginx等反代分流。

**对接vmess+tcp+http**

原生V2board不支持tcp+http订阅下发，请自行寻找解决方法，或手动配置客户端文件。

在对接vmess+tcp+http时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "header": {
    "type": "http",
    "request": {},
    "response": {}
  }
}
```

其中`request`和`response`中的内容请自行参照[Xray-core文档](https://xtls.github.io/config/transports/tcp.html#httpheaderobject)设置。

![image-20221113122646413](tp/jdaz.tp/image-20221113122646413.png)

## 3. 节点安装XrayR后端

用ssh连接节点
使用一键脚本安装XrayR（[Github地址](https://github.com/XrayR-project/XrayR)）

```shell
bash <(curl -Ls https://raw.githubusercontent.com/XrayR-project/XrayR-release/master/install.sh)
```

或者通过[Docker安装](https://crackair.gitbook.io/xrayr-project/xrayr-xia-zai-he-an-zhuang/install/docker)

配置文件路径：`/etc/XrayR`

## 4. 配置XrayR

```shell
vi /etc/XrayR/config.yml
```

添加节点（根据自己需求改，这里只显示修改的地方，[官方详细文档](https://crackair.gitbook.io/xrayr-project/xrayr-pei-zhi-wen-jian-shuo-ming/config)）

```shell
PanelType: "V2board"
    ApiConfig:
      ApiHost: "https://plane.mrjiang.top" # V2Board面板地址
      ApiKey: "**********" # V2Board后台面板-系统配置-服务端-通讯密钥
      NodeID: 1 #节点ID，添加节点时显示
      NodeType: V2ray 
      CertConfig:
        CertMode: file
        CertDomain: "jp.mrjiang.top"
        CertFile: /etc/XrayR/cert/jp.mrjiang.top.cert
        KeyFile: /etc/XrayR/cert/jp.mrjiang.top.key
```

![image-20221113122928099](tp/jdaz.tp/image-20221113122928099.png)完毕

## 5.完毕

到此已经可以正常使用了，其他诸如支付、套餐等配置，根据说明来
[V2Board使用手册](https://docs.v2board.com/)