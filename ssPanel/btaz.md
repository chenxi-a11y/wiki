## 1.环境准备

SSPanel UIM 的需要以下程序才能正常的安装和运行：

- Git

- PHP 8.0+

- Composer

- MySQL / MariaDB

SSPanel UIM 支持安装在 LNMP、CloudPanel 等集成环境中。

在正式安装前可以用`ls /usr/bin/composer`来检查下环境是否都准备好了，宝塔是默认安装过composer的

报错的话注意升级下试试`composer selfupdate`

当然如果还是不行可以排查下是不是默认php版本不对

Plain  Text

  rm -f /usr/bin/php

删除默认的配置

将默认版本修改成你正在使用的php版本

Plain  Text

  ln -sf /www/server/php/80/bin/php /usr/bin/php

确定好后环境就没问题了，咱们就可以开始了，因为大部分都是宝塔用户，所以这里就拿宝塔举例子~



**开始安装**

## 2.创建一个站点

![image-20220924135949000](tp/btaz.tp/image-20220924135949000.png)

需要注意的就这2个地方，域名处填写你的域名即可（好像是废话 ）数据库不需要，后面再自建

## 3.拉取源码

删除网站目录默认文件

在宝塔面板里打开网站目录

![image-20220924140139846](tp/btaz.tp/image-20220924140139846.png)

全选删除

<img src="tp/btaz.tp/image-20220924140222990.png" alt="image-20220924140222990" style="zoom: 50%;" />

但是.user.ini无法删除，这时回到ssh控制台

**删除.user.ini**

确保当前ssh终端目录在网站根目录，执行

```bash
// 修改权限 
chattr -i . user.ini 
// 再次删除 
rm -rf .user.ini
```

进入到站点根目录

```bash
cd /www/wwwroot/你的网站目录
```

下载最新版的代码

```bash
git clone https://github.com/Anankke/SSPanel-Uim.git .
```

如果提示找不到git命令请安装git

```
yum -y install gi
```

这里建议去下载最新版上传到站点目录去解压。。 下面两个是源码下载地址： 

更新记录：[https://wiki.sspanel.org/#/release-note](https://wiki.sspanel.org/#/release-note?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2NjM5OTk3NTUsImZpbGVHVUlEIjoiZ08zb3hHb0tQYXNyNlZxRCIsImlhdCI6MTY2Mzk5OTQ1NSwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotNzE4NzE2ODk4OH0.A1H7GOPUClTjNHwpLFmFnIjGjTjt3ayphwUf2jRcDjM)

 [https://github.com/Anankke/SSPanel-Uim/releases](https://github.com/Anankke/SSPanel-Uim/releases?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2NjM5OTk3NTUsImZpbGVHVUlEIjoiZ08zb3hHb0tQYXNyNlZxRCIsImlhdCI6MTY2Mzk5OTQ1NSwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotNzE4NzE2ODk4OH0.A1H7GOPUClTjNHwpLFmFnIjGjTjt3ayphwUf2jRcDjM) 

## 4.设置伪静态

在网站-站点设置中

两种自己试试哪个能用

第一种

```
location /

{

  try_files $uri /index.php$is_args$args;

}
```

第二种

```
location / {

  try_files $uri $uri/ /index.php$is_args$args;

}


```

将网站目录的运行目录改为public，关闭防跨站。

## 5.其他必要设置

**将网站目录的运行目录改为public，关闭防跨站。**

![image-20220924142120562](tp/btaz.tp/image-20220924142120562.png)

**删除禁用函数**

然后进入，软件商店-php设置-禁用函数中，删除掉这几个函数

popen、proc_open、putenv

![image-20220924142238246](tp/btaz.tp/image-20220924142238246.png)

**##修改权限**

现在在你的站点根目录下找到 `storage` 目录，点击如图按钮修改权限为 `777`，并把所有者改为 `www`

![image-20220924142332133](tp/btaz.tp/image-20220924142332133.png)



## 6.安装面板

进入网站根目录

```bash
cd /www/wwwroot/你的网站目录
```

输入以下命令安装面板！

```bash
cp config/.config.example.php config/.config.php
cp config/appprofile.example.php config/appprofile.php
mv db/migrations/20000101000000_init_database.php.new db/migrations/20000101000000_init_database.php
wget https://getcomposer.org/installer -O composer.phar
php composer.phar
php composer.phar install
chmod 755 -R *
chown www -R *
git config --global --add safe.directory $(pwd)
git checkout .
```

**修改配置文件**

编辑文件 `config/.config.php` ，找到以下部分

- `db_host` 如果使用本地数据库，填 `localhost` 或 `127.0.0.1`

- 如果使用云数据库，填写 `ip` 或域名，并注意允许服务器 `ip` 连接

- `db_socket` 可留空，或根据文件上方注释填写

- 注意数据库账户需要有对表结构的操作权限

- 数据库名默认是 `sspanel` ，可修改为其他的。但注意后续创建数据库时，创建的库名需与在此填写的保持一致

!>（注意：这里是编辑文件 `config/.config.php` 不是执行ssh代码）

```PHP
$_ENV['db_driver'] = 'mysql';
$_ENV['db_host'] = '';
$_ENV['db_socket'] = '';
$_ENV['db_database'] = 'sspanel'; //数据库名，可自定义
$_ENV['db_username'] = 'root'; //数据库用户名，建议root
$_ENV['db_password'] = 'sspanel'; //用户名对应的密码，在宝塔面板数据库里查看
```

还需要依照注释，修改这些重要的参数

```PHP
$_ENV['key']        = '1145141919810';                //!!! 瞎 jb 修改此key为随机字符串确保网站安全 !!!
$_ENV['debug']      = false;                          //正式环境请确保为 false
$_ENV['appName']    = 'SSPanel-UIM';                      //站点名称
$_ENV['baseUrl']    = 'https://sspanel.host';               //站点地址
$_ENV['muKey']      = 'NimaQu';                       //用于校验魔改后端请求，可以随意修改，但请保持前后端一致，否则节点不能工作！
```

**创建数据库**

此时打开宝塔面板内的 数据库，新建一个数据库，配置要跟上面修改的一样

**导入设置**

```bash
vendor/bin/phinx migrate
php xcat Tool importAllSettings
php xcat Tool initQQwry
```

仅 `dev` 分支需要执行

```bash
php xcat Tool detectConfigs
```

创建管理员账户

```bash
php xcat User createAdmin
```

## 7.添加定时任务

**dev 分支**

```bash
cd /root
crontab -l > crontab.list

echo "
0 */1 * * * php /www/wwwroot/你的站点目录/xcat Job UserJob
*/1 * * * * php /www/wwwroot/你的站点目录/xcat Job SendMail
*/1 * * * * php /www/wwwroot/你的站点目录/xcat Job CheckJob
30 23 * * * php /www/wwwroot/你的站点目录/xcat SendDiaryMail
0 0 * * *   php -n /www/wwwroot/你的站点目录/xcat Job DailyJob
" >> crontab.list

crontab crontab.list
rm crontab.list
```

**new-feat 分支**

```bash
cd /root
crontab -l > crontab.list

echo "
0 */1 * * * php /www/wwwroot/你的站点目录/xcat Job UserJob
*/1 * * * * php /www/wwwroot/你的站点目录/xcat Job SendMail
*/1 * * * * php /www/wwwroot/你的站点目录/xcat Job CheckJob
0 0 * * * php -n /www/wwwroot/你的站点目录/xcat Job DailyJob
30 23 * * * php /www/wwwroot/你的站点目录/xcat SendDiaryMail
0 0 * * * php /www/wwwroot/你的站点目录/xcat Statistics Another
59 23 * * * php /www/wwwroot/你的站点目录/xcat Statistics CheckIn
" >> crontab.list

crontab crontab.list
rm crontab.list

```

**可选定时任务**

**财务报表**

```bash
5 0 * * * php /www/wwwroot/你的站点目录/xcat FinanceMail day 
6 0 * * 0 php /www/wwwroot/你的站点目录/xcat FinanceMail week
7 0 1 * * php /www/wwwroot/你的站点目录/xcat FinanceMail month
```



**crontab -e**

**按i进入输入模式**

**输入完之后按esc，然后输入:wq，按回车保存并退出。**



**网友分享的方法（不知道有没有用，自己试试吧）**

**crontab -e**

**按i进入输入模式**

```bash
echo "*/1 * * * * /usr/bin/php /www/wwwroot/你的站点目录/xcat Job SendMail
*/1 * * * * /usr/bin/php /www/wwwroot/你的站点目录/xcat Job CheckJob
0 */1 * * * /usr/bin/php /www/wwwroot/你的站点目录/xcat Job UserJob
30 23 * * * /usr/bin/php /www/wwwroot/你的站点目录/xcat SendDiaryMail
0 0 * * *   /usr/bin/php -n /www/wwwroot/你的站点目录/xcat Job DailyJob" >> crontab.list
```

**财务报表（可选）**

```bash
5 0 * * * /usr/bin/php /www/wwwroot/你的站点目录/xcat FinanceMail day 
6 0 * * 0 /usr/bin/php /www/wwwroot/你的站点目录/xcat FinanceMail week
7 0 1 * * /usr/bin/php /www/wwwroot/你的站点目录/xcat FinanceMail month
```

**输入完之后按esc，然后输入:wq，按回车保存并退出。**



## 8.使用宝塔面板的计划任务配置

```shell
用户账户相关任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每小时
脚本内容：php /www/wwwroot/你的网站目录/xcat Job UserJob

定时检测邮件队列 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job SendMail

检测任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job CheckJob

每日流量报告 (给开启每日邮件的用户发送邮件)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SendDiaryMail

每日任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job DailyJob

SSPanel任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Statistics Another

SSPanel任务2 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Statistics CheckIn


下面这些不添加也可以

检查用户会员等级过期任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat Job CheckUserClassExpire

检查账号过期任务 (必须)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每小时
脚本内容：php /www/wwwroot/你的网站目录/xcat Job CheckUserExpire

审计封禁 (建议设置)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat DetectBan

检测节点被墙 (可选)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat DetectGFW

检测中转服务器 (可选)
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 5 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat DetectTransfer

Radius (可选)
synclogin
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius synclogin

syncvpn
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius syncvpn

syncnas
任务类型：Shell 脚本
任务名称：自行填写
执行周期：N分钟 1 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat SyncRadius syncnas
自动备份 (可选)

整体备份
任务类型：Shell 脚本
任务名称：自行填写
执行周期：自己设置, 可以设置每30分钟左右
脚本内容：php /www/wwwroot/你的网站目录/xcat Backup full

只备份核心数据
任务类型：Shell 脚本
任务名称：自行填写
执行周期：自己设置, 可以设置每30分钟左右
脚本内容：php /www/wwwroot/你的网站目录/xcat Backup simple
财务报表 (可选)

日报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每天 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail day

周报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每星期 周日 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail week

月报
任务类型：Shell 脚本
任务名称：自行填写
执行周期：每月 1 日 0 小时 0 分钟
脚本内容：php /www/wwwroot/你的网站目录/xcat FinanceMail month
```

至此，前端面板就安装就完成了



## 9建议

**安全**

如需在生产模式开启 `debug` 模式，可以执行 `bash block-whoops-env.sh` ，便能将敏感的环境参数隐藏

执行 `bash block-whoops-env.sh recover` 可以使用备份恢复修改的文件



**备份**

执行以下命令导出数据库文件，建议配合其他脚本或工具备份到云端

```bash
mysqldump -h127.0.0.1 -uroot -p刚才获取的数据库密码 --databases sspanel > sspanel.sql
```



**同步更新**

**dev 分支**

```bash
git pull
```

**new-feat**[ ](https://wiki.sspanel.org/#/install-using-cloudpanel-v2?id=new-feat-分支-2&accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2NjM5OTk3NTUsImZpbGVHVUlEIjoiZ08zb3hHb0tQYXNyNlZxRCIsImlhdCI6MTY2Mzk5OTQ1NSwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotNzE4NzE2ODk4OH0.A1H7GOPUClTjNHwpLFmFnIjGjTjt3ayphwUf2jRcDjM)**分支**

```bash
git pull origin new-feat:new-feat
```

有时候，光这么做可能不够，你可能还需要

```bash
composer update
vendor/bin/phinx migrate
php xcat Tool importAllSettings
```

同时需要注意有没有什么参数在 `.config.example.php` 文件中有，而在你的 `.config.php` 文件中没有的（你可以谷歌一些在线文本比对工具来方便排查）