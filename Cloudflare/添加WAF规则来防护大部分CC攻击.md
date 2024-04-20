# Cloudflare的客户可以添加 WAF 规则来防护大部分CC攻击

使用 Cloudflare 的客户可以添加 WAF 规则来防护大部分 CC 攻击

Cloudflare WAF 防护规则表达式

```
 (ip.geoip.country eq "T1") or (not http.request.method in {"GET" "POST" "PURGE" "PUT" "HEAD" "OPTIONS" "DELETE" "PATCH"})
```