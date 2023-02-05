# 宝塔面板安装云锁nginx自编译防护后网站访问异常解决方法

部分站长在云锁群内反馈，在宝塔面板安装云锁自编译后，会出现如下异常：

1.网站加载明显变慢；
2.部分情况下无法打开网站（链接被重置、关闭）
3.Nginx报错**worker process xxx exited on signal 6

当Nginx报错达到一定数量，服务会被终止，导致大量链接被断开，根据本站调研，此问题只会出现在流量比较大的网站上，流量低的网站不会出现此问题，解决方法：

打开以下文件：/usr/local/yunsuo_agent/FilterKernel.xml

找到：

```solidity
<PlugIn dllpath="libs/libperformanceMonitor.so" RunOn_Filter=".*" RunOn_Product=".*"/>
```

将这一行注释掉即可，xml的注释方式为：

```
<!--要注释的内容-->
```

注意事项：

1.修改前需要先关闭云锁的“自身防护”功能。
2.修改后需要重启nginx，请先评估重启nginx对业务带来的影响。