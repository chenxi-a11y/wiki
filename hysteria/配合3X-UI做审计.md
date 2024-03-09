**Debian12系统**

## 1.安装3X-UI

https://github.com/MHSanaei/3x-ui

在出站规则  outbounds 处加以下配置

```
{
    "protocol": "blackhole",
    "tag": "block"
    }
```


**上面block标签的走blackhole,代表屏蔽的意思。**

在路由规则  routing 处加以下配置

```
{
      "type": "field",
      "outboundTag": "block",
      "domain": [
        "regexp:(api|ps|sv|offnavi|newvector|ulog.imap|newloc)(.map|).(baidu|n.shifen).com",
        "regexp:(.+.|^)(360|so).(cn|com)",
        "regexp:(Subject|HELO|SMTP)",
        "regexp:(torrent|.torrent|peer_id=|info_hash|get_peers|find_node|BitTorrent|announce_peer|announce.php?passkey=)",
        "regexp:(^.@)(guerrillamail|guerrillamailblock|sharklasers|grr|pokemail|spam4|bccto|chacuo|027168).(info|biz|com|de|net|org|me|la)",
        "regexp:(.?)(xunlei|sandai|Thunder|XLLiveUD)(.)",
        "regexp:(..||)(dafahao|mingjinglive|botanwang|minghui|dongtaiwang|falunaz|epochtimes|ntdtv|falundafa|falungong|wujieliulan|zhengjian).(org|com|net)",
        "regexp:(ed2k|.torrent|peer_id=|announce|info_hash|get_peers|find_node|BitTorrent|announce_peer|announce.php?passkey=|magnet:|xunlei|sandai|Thunder|XLLiveUD|bt_key)",
        "regexp:(.+.|^)(360|fast|so).(cn|com|net)",
        "regexp:(.*.||)(rising|kingsoft|duba|xindubawukong|jinshanduba).(com|net|org)",
        "regexp:(.*.||)(netvigator|torproject).(com|cn|net|org)",
        "regexp:(..||)(visa|mycard|mastercard|gov|gash|beanfun|bank).",
        "regexp:(.*.||)(gov|12377|12315|talk.news.pts.org|creaders|zhuichaguoji|efcc.org|cyberpolice|aboluowang|tuidang|epochtimes|nytimes|zhengjian|110.qq|mingjingnews|inmediahk|xinsheng|breakgfw|chengmingmag|jinpianwang|qi-gong|mhradio|edoors|renminbao|soundofhope|xizang-zhiye|bannedbook|ntdtv|12321|secretchina|dajiyuan|boxun|chinadigitaltimes|dwnews|huaglad|oneplusnews|epochweekly|cn.rfi).(cn|com|org|net|club|net|fr|tw|hk|eu|info|me)",
        "regexp:(.*.||)(miaozhen|cnzz|talkingdata|umeng).(cn|com)",
        "regexp:(.*.||)(mycard).(com|tw)",
        "regexp:(.*.||)(gash).(com|tw)",
        "regexp:(.bank.)",
        "regexp:(.*.||)(pincong).(rocks)",
        "regexp:(api|ps|sv|offnavi|newvector|ulog.imap|newloc)(.map|).(baidu|n.shifen).com",
        "regexp:(.+.|^)(360|so).(cn|com)",
		"regexp:(Subject|HELO|SMTP)",
		"regexp:(.*.||)(sinaimg).(cn|com|net)",
		"regexp:(..||)(dafahao|mingjinglive|botanwang|minghui|dongtaiwang|falunaz|epochtimes|ntdtv|falundafa|falungong|wujieliulan|zhengjian).(cn|com|org|net|club|net|fr|tw|hk)",
		"regexp:(.*.||)(gov|12377|12315|talk.news.pts|zhuichaguoji|efcc|cyberpolice|tuidang|nytimes|falundafa|falunaz|110.qq|mingjingnews|inmediahk|xinsheng|12321|epochweekly|cn.rfi|mingjing|chinaaid|botanwang|xinsheng|rfi|breakgfw|chengmingmag|jinpianwang|xizang-zhiye|breakgfw|qi-gong|voachinese|mhradio|rfa|edoors|edoors|renminbao|soundofhope|zhengjian|dafahao|minghui|dongtaiwang|epochtimes|ntdtv|falundafa|wujieliulan|aboluowang|bannedbook|secretchina|dajiyuan|boxun|chinadigitaltimes|huaglad|dwnews|creaders|oneplusnews|rfa|nextdigital|pincong|gtv|kwok7).(cn|com|org|net|club|net|fr|tw|hk|eu|info|me|rocks)",
		"regexp:(.*.||)(64tianwang|beijingspring|boxun|broadpressinc|chengmingmag|chenpokong|chinaaffairs|chinadigitaltimes|chinesepen|dafahao|dalailamaworld|dalianmeng|dongtaiwang|epochweekly|erabaru|fgmtv|hrichina|huanghuagang|hxwq|jiangweiping|lagranepoca|lantosfoundation|minghui|minzhuzhongguo|ned|ninecommentaries|ogate|renminbao|rfa|secretchina|shenyun|shenyunperformingarts|shenzhoufilm|soundofhope|tiantibooks|tibetpost|truthmoviegroup.wixsite|tuidang|uhrp|uyghuramerican|voachinese|vot|weijingsheng|wujieliulan|xizang-zhiye|zhengjian|zhuichaguoji).(org|com|net)",
		"regexp:(.*.||)(gov|12377|12315|talk.news.pts|zhuichaguoji|efcc|cyberpolice|tuidang|nytimes|falundafa|falunaz|110.qq|mingjingnews|inmediahk|xinsheng|12321|epochweekly|cn.rfi|mingjing|chinaaid|botanwang|xinsheng|rfi|breakgfw|chengmingmag|jinpianwang|xizang-zhiye|breakgfw|qi-gong|voachinese|mhradio|rfa|edoors|edoors|renminbao|soundofhope|zhengjian|dafahao|minghui|dongtaiwang|epochtimes|ntdtv|falundafa|wujieliulan|aboluowang|bannedbook|secretchina|dajiyuan|boxun|chinadigitaltimes|huaglad|dwnews|creaders|oneplusnews|rfa|nextdigital|pincong|gtv|kwok7).(cn|com|org|net|club|net|fr|tw|hk|eu|info|me|rocks)",
		"regexp:(torrent|.torrent|peer_id=|info_hash|get_peers|find_node|BitTorrent|announce_peer|announce.php?passkey=)",
		"regexp:(^.@)(guerrillamail|guerrillamailblock|sharklasers|grr|pokemail|spam4|bccto|chacuo|027168).(info|biz|com|de|net|org|me|la)",
		"regexp:(^.*@)(guerrillamail|guerrillamailblock|sharklasers|grr|pokemail|spam4|bccto|chacuo|027168).(info|biz|com|de|net|org|me|la)",
		"regexp:(.*.||)(sigmapool|hashcity|2miners|solo-etc|nanopool|minergate|comining|give-me-coins|hiveon|arsmine|baikalmine|solopool|litecoinpool|mining-dutch|clona|viabtc|beepool|maxhash|bwpool|coinminerz|miningcore|multipools|uupool|minexmr|pandaminer|f2pool|sparkpool|antpool|poolin|slushpool|marathondh|pool.btc).(cn|com|org|net|club|net|fr|tw|hk|eu|info|me|io)",
		"regexp:(.+.|^)(360|so|qihoo|360safe|qhimg|360totalsecurity|yunpan).(cn|com)",
		"regexp:(api|ps|sv|offnavi|newvector|ulog|newloc).(map|imap).(baidu|n.shifen).com",
		"regexp:(.*.)(metatrader4|metatrader5|mql5).(org|com|net)",
		"regexp:(^.*@)(guer­ril­la­mail|guer­ril­la­mail­block|shark­lasers|grr|poke­mail|spam4|bc­cto|chacuo|027168).(info|biz|com|de|net|org|me|la)",
		"regexp:(.+.|^)(whatismyip|whatismyi­pad­dress|ipip|iplo­ca­tion|myip|whatismy­browser).(cn|com|net|com|net­work) (.+.|^)(whatismyip|whatismyi­pad­dress|ipip|iplo­ca­tion|myip|whatismy­browser).(cn|com|net|com|net­work)",
		"regexp:(c)(.tcbox|wappass|nsclick|sofire|gips0|afd|als|hmma|info|bgg|mbd|afdconf|).(tuisong|baidu|bdstatic).(cn|com|net)",
		"regexp:(.+.|^)(zhuanzhuan|pinduoduo|kskwai|kwaizt|gifshow|kuaishouzt|kwimgs|yximgs|ksapisrv|kuaishou|autonavi|xfinfr).(cn|com|net)",
		"regexp:(.+.|^)(zhihu).(com)",
		"regexp:(.*.||)(xiaohongshu|xhscdn).(cn|com|net)",
		"regexp:(.+.|^)(amemv|ecombdapi|toutiao|baike|zijieapi|douyinpic|bytedance|pstatp|bdurlsnssdk|awemueughun|oceanengine|douyinstatic).(cn|com|net)",
		"regexp:(.*.||)(dafa­hao|minghui|dong­tai­wang|epochtimes|nt­dtv|falundafa|wu­jieli­u­lan|zhengjian).(org|com|net)",
		"regexp:(.*.||)(antpool|foundrydigital|f2pool|viabtc|mining-dutch|solopool|hiveon|minergate|comining|give-me-coins|arsmine|baikalmine|litecoinpoo|clona|btc|slushpool|pandaminer|beepool|maxhash|coinminerz|bwpool|poolin|uupool|miningcore|multipools|minexmr|sbicrypto|marathondh|okex|emcd|luxor|sigmapool|kucoin|okkong|hpt|minerium|ckpool|mmpool|hashcity|huobipool|coinex|sparkpool|qkl123|coingecko|2miners|51szzc|666pool|91pool|atticpool|anomp|aapool|antpool|ash-shanghai.globalpool|asia.zcoin.miningpoolhub|blackpool|blockmasters|btchd|bitminter|bitcoin|bhdpool|bginpoolbaimin|bi-chi|bohemianpool|bixin|bwpool|btcguild|batpool|bw|btcc|btc|bitfury|bitclubnetwork|beepool|coinhive|chainpool|connectbtc|cybtc|canoepool|cryptograben|cryptonotepool|coinotron|dashcoinpool|dxpool|dwarfpool|dpool|dmpools|everstake|epool|ethpool|ethfans|easy2mine|ethermine|extremepool|firepool|fir|fkpool|flypool|f3pool|gridcash|gath3r|grin-pool|grinmint|gbminers|get.bi-chi|globalpool|give-me-ltc|honeyminer|honestmining|hashquark|hashrabbit|hummerpool|hdpool|h-pool|hashvault|hpool|huobipool|haopool|pool.btc).(com|cn|net|org|io|im|cc|pro|top|one|co|info)",
		"regexp:(.*.||)(cachefly).(org|com|net|de)",
		"regexp:(.*.||)(metatrader4|metatrader5|mql5).(org|com|net)",
		"regexp:(eth|asia|eth-eu|eth-us|cn|eth-backup|eth-na|stratum-etheth-eu1|eth-eu2).(antpool|sparkpool|f2pool|nanopool).(org|com)",
		"regexp:(.*.||)(gash).(com|tw)",
		"regexp:(.*.||)(mycard).(com|tw)",
		"regexp:(.*.||)(cyberpolice|12377|110|12337|12389|jubao|8221110|81|12388|isc|12339|js12377).(org|com|net|cn|gov)",
		"regexp:(.*.||)(metatrader4|metatrader5|mql5).(org|com|net)",
		"regexp:(.*.||)(netvigator|torproject).(com|cn|net|org)",
		"regexp:(.*.||)(rising|kingsoft|duba|xindubawukong|jinshanduba).(com|net|org)",
		"regexp:(.*.||)(adsafe).(com)",
		"regexp:(.*.||)(ris­ing|king­soft|duba|xin­dubawukong|jin­shan­duba).(com|net|org)",
		"regexp:(.*.||)(netvi­ga­tor|tor­pro­ject).(com|cn|net|org)",
		"regexp:(.gov.)",
		"regexp:(.go.kr.)",
		"regexp:(.*.||)(bank|icbc|ccb|abchina|boc|cmbchina|psbc|cib|cmbc|pingan|hxb|cgbchina|jsbchina|nbcb|njcb|cqrcb|srcb|cbhb|csbchina|gdrcb|bjrcb|xib|tccb|hrbb|cdrcb|szrcb|klb|sdb|bosc|tjrcb|qrcb|qlbchina|hkbchina|nhrcb|wzcb|czcb|msbc|fdb|bob|csccb|whccb|cnbhx|xsrcb|nyyb|cq3q|fsny).(cn|com|com.cn)",
		"regexp:(.*.||)(tuidang.ddns|xinsheng|pincong|pages|newhighlandvision).(cn|com|org|net|club|net|fr|tw|hk|rocks|dev)",
        "regexp:(.*.||)(taobao).(com)"
      ]
      },
      {
      "type": "field",
      "outboundTag": "block",
      "ip": [
          "127.0.0.1/32",
          "10.0.0.0/8",
          "fc00::/7",
          "fe80::/10",
          "172.16.0.0/12"
      ]
      },
      {
      "type": "field",
      "outboundTag": "block",
      "protocol": ["bittorrent"]
      },
      {
      "type": "field",
      "outboundTag": "block",
      "port": "22,23,24,25,107,194,445,465,587,992,3389,6665-6669,6679,6697,6881-6999,7000,10000-65535"
      }
```

**domain指定的域名，protocol是bittorrent，端口是22等的都标记为block标签。**

## 2.然后添加一个 socks5 代理



**hysteria的配置**

```
v2board:
  apiHost: http://20.24.34.102:81
  apiKey: dhrdkredsekj45753fdk
  nodeID: 2
tls:
  type: tls
  cert: /etc/letsencrypt/live/jpcs.域名.me/fullchain.pem
  key: /etc/letsencrypt/live/jpcs.域名.me/privkey.pem
auth:
  type: v2board
trafficStats:
  listen: 127.0.0.1:7653
outbounds:
  - name: direct
    type: direct
    direct:
      mode: auto
  - name: proxy
    type: socks5
    socks5:
      addr: 127.0.0.1:8080   // 3x-ui添加的代理 注意不要开密码
acl:
  inline: 
    - reject(10.0.0.0/8)
    - reject(172.16.0.0/12)
    - reject(192.168.0.0/16)
    - reject(127.0.0.0/8)
    - reject(fc00::/7)
    - proxy(all)
```



**使用acl控制 proxy(all) 全局走 proxy**



## 3.使用iptables 限制socks5代理只允许本机连接

一、安装iptables
1.安装iptables

```
sudo apt-get update

sudo apt-get install iptables
```


安装完成后，可以使用以下命令检查iptables是否已成功安装：

```
sudo iptables --version
```

如果输出iptables的版本号，则表示iptables已成功安装。



必须安装不然无法保存规则
1. 安装软件包

```
sudo apt install ipset iptables netfilter-persistent ipset-persistent iptables-persistent
```



2. 启用配置持久保存功能

```
sudo systemctl enable netfilter-persistent

sudo systemctl start netfilter-persistent
```



三、然后，使用以下命令添加规则，允许SSH连接：
这一步可以跳过

```
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```


四、添加黑白名单

先限制所有IP访问8080端口

i

```
ptables -I INPUT -p tcp -m multiport --dport 8080  -j DROP
iptables -I INPUT -p udp -m multiport --dport 8080  -j DROP
```


再添加单个IP白名单，要允许多个IP则添加多条即可

```
iptables -I INPUT -s 127.0.0.1 -p tcp -j ACCEPT
iptables -I INPUT -s 127.0.0.1 -p udp -j ACCEPT
```


六、保存一下ipset规则 如果不进行以下保存操作，重启之后规则会失效

```
netfilter-persistent save
```

重启防火墙
service iptables restart

1.输入以下命令查看到每个规则chain的序列号。

```
iptables -L -n --line-number
```

