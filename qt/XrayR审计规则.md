# XrayR审计规则

禁止BT禁止端口和网站等

[不懂的看xray的文档](https://xtls.github.io/document/level-1/routing-lv1-part1.html#_1-初识【路由】三兄弟)，看得懂xray就可以看懂了。

默认屏蔽这些端口22,23,24,25,107,194,445,465,587,992,3389,6665-6669,6679,6697,6881-6999,7000,10000-65535

## 1.把下面规则粘贴到xrayR的route.json里面

```yaml
{
  "domainStrategy": "IPOnDemand",
  "rules": [
    {
      "type": "field",
      "outboundTag": "block",
      "ip": [
        "geoip:private"
      ]
    },
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
        "regexp:(.+.|^)(360|speedtest|fast|so).(cn|com|net)",
        "regexp:(.*.||)(guanjia.qq.com|qqpcmgr|QQPCMGR)",
        "regexp:(.*.||)(rising|kingsoft|duba|xindubawukong|jinshanduba).(com|net|org)",
        "regexp:(.*.||)(netvigator|torproject).(com|cn|net|org)",
        "regexp:(..||)(visa|mycard|mastercard|gov|gash|beanfun|bank).",
        "regexp:(.*.||)(gov|12377|12315|talk.news.pts.org|creaders|zhuichaguoji|efcc.org|cyberpolice|aboluowang|tuidang|epochtimes|nytimes|zhengjian|110.qq|mingjingnews|inmediahk|xinsheng|breakgfw|chengmingmag|jinpianwang|qi-gong|mhradio|edoors|renminbao|soundofhope|xizang-zhiye|bannedbook|ntdtv|12321|secretchina|dajiyuan|boxun|chinadigitaltimes|dwnews|huaglad|oneplusnews|epochweekly|cn.rfi).(cn|com|org|net|club|net|fr|tw|hk|eu|info|me)",
        "regexp:(.*.||)(miaozhen|cnzz|talkingdata|umeng).(cn|com)",
        "regexp:(.*.||)(mycard).(com|tw)",
        "regexp:(.*.||)(gash).(com|tw)",
        "regexp:(.bank.)",
        "regexp:(.*.||)(pincong).(rocks)",
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
  ]
}
```

domain指定的域名，protocol是bittorrent，端口是22等的都标记为block标签，

![photo_2021-06-13_00-51-35](./tp/XrayR审计规则.tp/photo_2021-06-13_00-51-35-220x150.jpg)

## 2.custom_outbound.json文件写如这个：

```yaml
[
  {
    "tag": "IPv4_out",
    "protocol": "freedom",
    "settings": {}
  },
  {
    "tag": "IPv6_out",
    "protocol": "freedom",
    "settings": {
      "domainStrategy": "UseIPv6"
    }
  },
  {
    "protocol": "blackhole",
    "tag": "block"
  }
]
```

上面block标签的走blackhole,代表屏蔽的意思。

## 3.config.yml

RouteConfigPath和OutboundConfigPath 后面的#号去除，开启指定路由规则。