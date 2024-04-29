# Linux VPS系统设置时区和同步时间的简单方法

**说明：**有时候vps时区和时间不一样会出很多问题，这里就不举例了，只说下方法。

**1、修改北京时区** 这里以修改北京时间作为默认时区，如果有其他需要的，可以对应修改。

```javascript
rm -rf /etc/localtime #先删除默认的时区设置
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime #替换上海/北京作为默认
```

**2、手工修改当前系统的时间**

```javascript
date -s '14:48:00 2015-05-10'
```

这里，就修改为当前的时间。

**3、设置同步时间**

```javascript
ntpdate us.pool.ntp.org
```

设置同步[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)时间，安装完毕之后，我们用`date`测试下当前时间。

一般的`VPS`都有安装`NTP`，如果没有安装我们需要先安装`yum install -y ntp`。

总结，这样我们通过上面的3步骤就快速的实现`Linux VPS`系统时间与当前需要的时区和时间同步一致，确保项目的正常运行。