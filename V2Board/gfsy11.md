# 更新升级

V2Board强依赖git，所以你如果要进行操作请务必是使用git的形式进行部署。

## 升级至稳定版

站点目录下执行

```bash
sh update.sh
```

## 升级至开发版

请注意升级到开发版本后无法退回，升级前请务必做好备份做事以便回滚

```bash
sh update_dev.sh
```

⚠️如果有额外要求需要重启队列服务，您可以对您队列服务的守护服务进行重启，例如PM2或Supervisor。

## 备份重装

备份数据库及 config/v2board.php 文件后全新安装v2board恢复文件即可完成重装。站点目录下执行 

```
php artisan config:cache
```



## 常见问题

Q：升级后版本号没变，升级失败。这里参考两位网友的解决方案   
A：<br>    1.

```
1.6.0运行更新脚本无效解决：
假如网站目录在/www/wwwroot/test

cd /www/wwwroot/test

git config --global --add safe.directory /www/wwwroot/test
然后重新运行更新
```

​    2.

```
我是这么解决的 

运行git tag查看版本号，发现提示detected dubious ownership

然后运行 git config --global --add safe.directory 网站目录
```

Q：更新完成后没有变化？   
A：如果你使用的是CDN请检查清除CDN缓存。

Q：非git部署如何更新？   
A：你可以到仓库下载最新版本的代码包全量覆盖更新。覆盖后请务必执行 

```
php artisan v2board:update
```

Q：如何降级？   
A：降级前必须要有数据库备份，否则将会导致问题。重装指定版本，导入指定版本数据库。