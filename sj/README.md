**常用审计规则**

```
1、禁用BT 防止版权争议（数据包明文匹配）

BitTorrent protocol

2、禁止百度高精度定位 防止IP与客户端地理位置被记录（数据包明文匹配）

(api|ps|sv|offnavi|newvector|ulog\.imap|newloc)(\.map|)\.(baidu|n\.shifen)\.com

3、禁止360服务 屏蔽360（数据包明文匹配）

(.+\.|^)(360|so)\.(cn|com)

4、禁止邮件滥发 防止垃圾邮件滥用（数据包明文匹配）

(Subject|HELO|SMTP)

5、屏蔽轮子网站（数据包明文匹配）

(.*\.||)(dafahao|minghui|dongtaiwang|epochtimes|ntdtv|falundafa|wujieliulan|zhengjian)\.(org|com|net)

6、屏蔽 BT（2）（数据包明文匹配）

(torrent|\.torrent|peer_id=|info_hash|get_peers|find_node|BitTorrent|announce_peer|announce\.php\?passkey=)

7、屏蔽Spam邮箱（数据包明文匹配）

(^.*\@)(guerrillamail|guerrillamailblock|sharklasers|grr|pokemail|spam4|bccto|chacuo|027168)\.(info|biz|com|de|net|org|me|la)

8、屏蔽迅雷（数据包明文匹配）

(.?)(xunlei|sandai|Thunder|XLLiveUD)(.)

屏蔽金融诈骗	(华为应用市场没网)

.bank.

屏蔽金融诈骗	

(.*.||)(gash).(com|tw)

屏蔽金融诈骗	

(.*.||)(mycard).(com|tw)
```

**使用**

```
BitTorrent protocol
(api|ps|sv|offnavi|newvector|ulog\.imap|newloc)(\.map|)\.(baidu|n\.shifen)\.com
(.+\.|^)(360|so)\.(cn|com)
(Subject|HELO|SMTP)
(.*\.||)(dafahao|minghui|dongtaiwang|epochtimes|ntdtv|falundafa|wujieliulan|zhengjian)\.(org|com|net)
(torrent|\.torrent|peer_id=|info_hash|get_peers|find_node|BitTorrent|announce_peer|announce\.php\?passkey=)
(^.*\@)(guerrillamail|guerrillamailblock|sharklasers|grr|pokemail|spam4|bccto|chacuo|027168)\.(info|biz|com|de|net|org|me|la)
(.?)(xunlei|sandai|Thunder|XLLiveUD)(.)
(.*.||)(gash).(com|tw)
(.*.||)(mycard).(com|tw)
google.com
```

