# Windows 本地部署教程

## 准备工具

- Windows

- Socks5 代理工具如 Clash、V2Ray 等等

- Telegram api_id、api_hash [获取地址](https://www.mspace.cc/?golink=aHR0cHM6Ly9teS50ZWxlZ3JhbS5vcmcvYXBwcw==)

- Telegram Bot Token [获取地址](https://www.mspace.cc/?golink=aHR0cHM6Ly90ZWxlZ3JhbS5tZS9Cb3RGYXRoZXI=)

- Python3.9+ [Python 3.10.10](https://www.python.org/ftp/python/3.10.10/python-3.10.10-amd64.exe)

- Windows PowerShell

  

## 下载工具

下载 Python 3.10.10，勾选加载环境并安装

![图片[4]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041733729.webp)

**安装完毕后重启电脑，非必须，但可以节省大量排错时间**

下载 FullTClash [最新版]([Releases · AirportR/fulltclash (github.com)](https://github.com/AirportR/fulltclash/releases)) 程序，我这里演示的是 3.6.2 版

![图片[6]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041738973.webp)

## 环境部署

### Python 环境

解压，`Control` + `R` 输入 `CMD `回车打开终端
把文件夹的路径复制一下，
![图片[7]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041746011.webp)

然后在终端输入 `CD` + `空格` + `右键` 按回车
![图片[8]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041749351.webp)

接着在终端输入以下命令并回车，快速部署环境

```
pip install -r requirements.txt
```

显示如图的时候就是环境安装完毕了
![图片[9]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041755303.webp)

## 电报机器人配置

进入 `resources` 文件夹，把 `config.yaml.examlp` 复制一份并改名 `config.yaml`
![图片[10]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041741561.webp)

打开 `config.yaml` 按照以下信息修改成自己的信息，主要分为管理员和机器人部分，在修改前先说说这些配置都怎么获取，如果你已经在别的地方获取过了可以跳过这部分

### 机器人信息获取方式

#### api_id、api_hash 获取方式

很多人大概率会卡在这一步，具体表现是无论怎么填写和操作都是提示错误
官方没有给出比较明确的解决办法，但我查阅资料和自己的测试来看，目前解决办法主要为

- 节点足够干净（无法确认）
- 电报注册 IP 和 操作 IP 为同一地区（未成功）
- 用国外 VPS 操作获取（成功）

最后我是用微软云（Azure）开了个日本主机才注册成功的（可退款）

![图片[12]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041814832.webp)

#### bot token 获取方式

去 [@BotFather]([Telegram: Contact @BotFather](https://t.me/BotFather)) 那里创建一个机器人
根据自己的要求取名，最后他会返回一个 token

![图片[13]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041820395.webp)

接着就可以把他们填入 config 里

### 配置填写

**管理员配置**

```
admin:

- 12345678 # 改成自己的telegram uid

- 想给谁用就加多一行他的 uid 或者 用户名
```

**bot 相关配置**

```
bot:

api_id: 123456 #改成自己的api_id

api_hash: 123456ABCDefg #改成自己的api_hash

bot_token: 123456:ABCDefgh123455  # bot_token, 从 @BotFather 获取
```

因为是部署在国内，程序还需要代理才能连接上 Telegram 服务器。回车加入如下信息：

```
proxy: 127.0.0.1:7890 #socks5 替换成自己的代理地址和端口
```

修改好后大概是这样，保存
![图片[14]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041822667.webp)

## 运行机器人

回到程序文件夹，双击 `main.py` 运行程序，如果出现以下页面就代表运行成功了
![图片[15]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041833852.webp)

## 使用机器人

打开电报，找到你的机器人（运行代码里有），给他发送 `/help` 就看到功能了
![图片[16]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041835698.webp)

接下来演示使用过程，详细的其他指令可以去看

### 流媒体解锁检测

**温馨提示：部分机场是禁止用户私自测速的，被发现轻则被警告重则直接封号，请确认自己已经经过机场主的同意再进行大流量的测速！**

输入以下指令把自己的机场添加进配置里（如果发现自己的节点为空，代表你可能需要进行订阅转换，把配置转成 Clash 文件再进行测速）

```
/new 节点地址 保存名称

如：

/new www.baidu.com 百度
```

添加成功后再用以下指令开始测速你的机场

```
/test 名称

如：

/test 百度
```

![图片[17]|最优雅的节点测速工具 FullTClash 流媒体解锁质量检测工具 | Windows 本地部署教程 | 牧之笔记 | 世界不应有局限](./tp/Windows部署.tp/202311041854890.webp)