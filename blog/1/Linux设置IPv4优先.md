# Linux设置IPv4优先

IPv6 VPS套了warp后变成了IPv4和IPv6双栈，如何设置IPv4优先呢？

以debain为例，直接修改 /etc/gai.conf 文件：

```
vi /etc/gai.conf
```

\#precedence ::ffff:0:0/96 100
把前面的”#”去掉即可。

或者：

```
echo "precedence ::ffff:0:0/96 100" >>/etc/gai.conf
```

