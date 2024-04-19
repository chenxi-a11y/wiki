官方文档：[FullTClash | FullTclash 文档 (gitbook.io)](https://fulltclash.gitbook.io/fulltclash-doc)

项目地址：[https://github.com/AirportR/fulltclash)](https://github.com/AirportR/fulltclash)

代理客户端：https://github.com/AirportR/FullTCore/releases

# 快速开始

首先需要准备以下信息：

- 

  Telegram 的api_id 、api_hash [获取地址](https://my.telegram.org/apps) 不会请Google。(部分TG账号已被拉黑，无法正常使用，尝试更换代理IP，IP干净成功率高，用机场节点就自求多福吧🙃)

- 

  去 [@BotFather](https://t.me/BotFather) 那里创建一个机器人，获得该机器人的bot_token，应形如：

  bot_token = "123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11"

  这步不会请Google。

###### 拉取源码

方法1：直接下载。

```
你可以在 [下载页面](https://github.com/AirportR/FullTclash/releases) 找到一些版本的源代码。
```

方法2：使用git（Linux推荐，方便更新），首先安装git，然后拉取仓库。以下指令为 Ubuntu 发行版作示例，Windows自行解决。

```
apt install -y git && git clone https://github.com/AirportR/FullTclash.git && cd FullTclash
```

此方法在中国大陆可能需要代理加速，请自行解决。

###### 环境准备

- 

  Python 3.9 以上(3.12暂时不推荐)

- 

  以及各种相关包依赖

您可以用以下命令，在当前项目目录下运行以快速安装环境：

> Windows:
>
> ```
> pip install -r requirements.txt
> ```

> Linux:
>
> ```
> pip3 install -r requirements.txt
> ```

###### 为bot进行相关配置

以下为启动bot的最低要求（如果您是新手，建议先以最低要求把bot跑起来，否则自己乱改配置容易出现不可预知的错误。）

- 

  管理员配置

  新建一个名为config.yaml的文件，放在./resources下，项目有模板例子名为./resources/config.yaml.example,在config.yaml中写入如下信息：

  ```
  admin:
  - 12345678 # 改成自己的telegram uid
  - 8765431 # 这是第二行，表示第二个管理员，没有第二个管理员就把该行删除。
  ```

- 

  bot相关配置

  ```
  bot:
   api_id: 123456 #改成自己的api_id
   api_hash: 123456ABCDefg #改成自己的api_hash
   bot_token: 123456:ABCDefgh123455  # bot_token, 从 @BotFather 获取
   # 如果是在中国大陆地区使用，则程序需要代理才能连接上Telegram服务器。写入如下信息：
   proxy: 127.0.0.1:7890 #socks5 替换成自己的代理地址和端口
  ```

- 

  代理客户端路径配置

从3.6.8开始，初次启动将自动下载以下(Windows,MacOS,Linux)(x86_64,arm64)的二进制文件，无需配置。

当然如果您想手动下载， 请自行前往以下网址获取: https://github.com/AirportR/FullTCore/releases 下载解压后可以放到 ./bin/ 目录下，比如文件名为 FullTCore ，下面的配置文件这样写：

```
clash:
 path: "./bin/FullTCore" #这里改成代理客户端文件路径
```

Windows系统名字后缀名.exe要加上，其他类Unix系统不需要加后缀名。

###### 获取session文件（可选）

您需要在项目文件目录下，放置一个已经登陆好的.session后缀文件，这个文件是程序生成的，形如： my_bot.session

> 方法1：可以直接在配置文件config.yaml中配置，这样程序启动后会自动读取配置文件里面的值来生成session文件(要求一定要正确)。

```
#配置文件示例，注意缩进要正确
bot:
 api_id: 123456
 api_hash: 123456ABCDefg
 bot_token: 123456:ABCDefgh123455
```

> 方法2： 您可以参阅[这篇文档](https://docs.pyrogram.org/start/auth)，以快速获得后缀为 .session 的文件

> 方法3： 项目的 ./utils/tool/ 目录下有一个文件名为 login.py ，可以通过指令运行它：
>
> ```
> python .\login.py
> ```

当程序退出后即可自动生成一个名为 my_bot.session 的文件 ，之后将它移动到项目根目录。 运行后它会尝试给你输入的用户名的目标发送消息，当接收到：嗨, 我在正常工作哦！

这句话时，即可说明该session文件有效，否则无效。

如果启动后无法验证，请删除生成的mybot.session文件，此时的文件是坏的，不可用，如果不删除程序会一直使用坏的文件，不会重新生成。

###### 开始启动

配置好后，在项目目录下运行以下指令

> Windows:
>
> ```
> python main.py
> ```

> Ubuntu(Linux):
>
> ```
> python3 main.py
> ```

等待初始化操作，出现“程序已启动!”字样就说明在运行了。 运行之后和bot私聊指令：

> /testurl <订阅地址>(clash配置格式)即可开始测试

> /help 可查看所有命令说明

更多用法请查阅文档。

###### 代理客户端编译(高级)

FullTClash有专用的代理客户端，存放在 ./bin/下。初次启动会自动帮您下载（仅限win、linux、darwin）对应平台的二进制文件。

文件的压缩包格式为: **FullTCore_{版本号}_{平台}_{CPU架构}.{压缩包后缀}**

没有所用架构？ 如果发现没有自动下载，说明没有在仓库中找到您所用架构的二进制文件，比如mips架构，那么您需要自行编译。

在 [此仓库](https://github.com/AirportR/FullTCore) 下有一源码文件为 fulltclash.go ，您需要将该文件自行用Golang编译器编译成二进制文件。

编译完成覆盖原文件即可 ，如果操作难度太大，可以发起issue详谈。

###### Docker启动

查看项目根目录的 ./docker/ 目录 ，有构建docker镜像的文件。

###### 持久化运行

您可以用 systemd , screen, pm2 等守护进程的解决方案持久化运行。

###### 交流探讨

我们欢迎各方朋友提出针对性的反馈：

- 

  [TG更新发布频道](https://t.me/FullTClash)

- 

  在项目页面提出issue

###### 致谢

- 

  [流媒体解锁检测思路](https://github.com/lmc999/RegionRestrictionCheck)

- 

  [Clash](https://github.com/Dreamacro/clash) ==> [mihomo](https://github.com/MetaCubeX/mihomo) [GPLv3]

- 

  [aiohttp](https://github.com/aio-libs/aiohttp) [Apache2]

- 

  [pyrogram](https://github.com/pyrogram/pyrogram) [LGPLv3]

- 

  [async-timeout](https://github.com/aio-libs/async-timeout) [Apache2]

- 

  [Pillow](https://github.com/python-pillow/Pillow) [HPND]

- 

  [pilmoji](https://github.com/jay3332/pilmoji) [MIT]

- 

  [pyyaml](https://github.com/yaml/pyyaml) [MIT]

- 

  [APScheduler](https://github.com/agronholm/apscheduler) [MIT]

- 

  [loguru](https://github.com/Delgan/loguru) [MIT]

- 

  [geoip2](https://github.com/maxmind/GeoIP2-python) [Apache2]

- 

  [cryptography](https://github.com/pyca/cryptography) [Apache2] [BSD3]

- 

  [google-re2](https://github.com/google/re2) [BSD3]

- 

  [aiohttp_socks](https://github.com/romis2012/aiohttp-socks) [Apache2]
