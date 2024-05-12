视频教程：https://bulianglin.com/archives/newcdn.html ，https://www.youtube.com/watch?v=NbruiJShUCE

**用到的工具**

CDN优选工具：https://bulianglin.com/archives/cdn.html
nodesCatch 节点测速工具：https://github.com/bulianglin/demo
搜索引擎：https://fofa.info
临时邮箱：http://24mail.chacuo.net
临时邮箱：https://www.linshiyouxiang.net
工具备份：https://github.com/chenxi-a11y/nodesCatch

**参考搜索语法**

```
国内反代IP：server=="cloudflare" && port=="80" && header="Forbidden" && country=="CN"
剔除CF：asn!="13335" && asn!="209242"
阿里云：server=="cloudflare" && asn=="45102"
甲骨文韩国：server=="cloudflare" && asn=="31898" && country=="KR"
搬瓦工：server=="cloudflare" && asn=="25820"

网站直接获取https://stock.hostmonit.com/CloudFlareYes
白嫖哥的：https://zip.baipiao.eu.org
CF中转IP发布 https://t.me/cf_push
```

**优选**

> 先来获取反代了CF IP的IP，用到的工具是之前经常出镜的fofa<br>
> 通过一些搜索语法就可以找到很多你想找的网站，详细搜索反代CF的IP可以参考我给大家提供的语法规则 **参考搜索语法**<br>
>
> server=="cloudflare" && port=="80" && header="Forbidden" && country=="CN" <br>
> 这条规则的意思是搜索服务器为CF，端口为80 ，HTTP应头部信息包含forbidden，地区为中国的IP<br>
> 粘贴到搜索框中 回车进行搜索
>
> 可以看到在这个条件下搜索到了2000多个独立IP，点击这里进行下载，提示我们需要先登录，点击去注册，，注册需要邮箱验证，可以在网上找一些临时邮箱<br>
> 按要求输入相关内容进行注册即可，点击注册，去邮箱查看激活邮件，如果等待超过1分钟还没有收到邮件，建议换一个节点IP重试，点击链接进行激活<br>
> 输入刚才注册的账号密码进行登录，登录成功后重新点击下载按钮，免费用户有2000多个导出额度，输入你要导出的数量，然后点击导出，进入个人中心进行下载<br>
> CSV格式的文件建议使用Excel表格打开 方便复制，我们要用到的就是这些IP，大部分都是反代了CF的IP，接下来就可以按照你习惯的方式进行优选了
>
> 也可以按照我提供的方法进行优选，保证是全网最快的方式<br>
> 首先打开这个网址进入CDN优选工具，如果打开之后跳转到了首页，可以在这个地方搜索CDN<br>
> 进入这个最佳的CDN优选工具，复制我们套了CDN的节点链接，注意地址栏应该是你的域名 而不是IP地址<br>
> 将节点链接粘贴到这里，CDN提供商选择自定义，选中后如果网页没有任何反应，可能是网页内容未完全加载<br>
> 复制我们刚才导出的IP，将其粘贴上去，获取节点数按需选择，刚才我们导出了2000条<br>
> 实际上会有重复的会自动去重，如果不知道具体的数量，可以填个大数，比如3000，保证每一条都会获取到<br>
> 点击提取节点，下方会生成所有反代IP的节点链接<br>
> 另外这个网页是纯前端JS脚本操作，我没有兴趣收集大家的节点信息，如果你有所顾虑，可以直接右键查看网页源代码，把源码扒下来在本地使用<br>
>
> 接下来跟着视频演示<br>
> 下载我之前做的节点测速工具，前两天顺便把401未授权的问题修了一下，下载之后将其解压出来<br>
> 运行目录下的nodesCatch可执行文件<br>
> 将刚才网页生成的节点全部复制，粘贴到工具中，2000条记录去重后还剩1600条<br>
> 先进行延迟测试<br>
> 至于这个工具的详细使用方式可以回看我之前的视频教程，这里就不花时间讲解了<br>
> Ctrl+a将所有节点全部选中，鼠标右键或者按Ctrl+r 进行延迟测试<br>
> 将无效的节点剔除，还剩700多个 有延迟说明一定可以使用，只是速度快慢的问题<br>
> 再来尝试测下载的速度，视频演示我就不全部测完了<br>
> 可以将速度还不错的节点，复制到其他代理工具中去使用<br>
> 可以看到节点的地址改成了反代CF的IP，伪装域名填入了我们套用CF的域名<br>
> 测速也是没有问题的 可以正常使用<br>
> 这就是套用优选反代CF IP 实现节点提速的效果<br>
> 不用整天盯着CF自家的IP去优选了<br>
>
> 虽然我们在fofa中筛选的是80端口的反代IP，但也有一部分IP既反代了80端口，也反代了443端口

