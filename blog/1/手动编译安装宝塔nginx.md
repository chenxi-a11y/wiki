# 手动编译安装宝塔nginx

手动编译安装宝塔nginx，以扩展一些其它模块，比如secure_link模块。

进入宝塔面板的安装文件夹： 打开终端，进入宝塔面板的安装目录，通常路径为 /www/server/panel/install/

```
cd  /www/server/panel/install/
wget http://download.bt.cn/install/0/nginx.sh

vim nginx.sh
```

在 nginx.sh 文件中，找到用于配置编译选项的部分（通常是以 ./configure 开头的行）。在该行的末尾添加：

```
--with-http_secure_link_module
```

以启用 secure_link 模块。

执行：

```
bash nginx.sh install 1.24
```

运行以下命令查看编译是否成功：

```
/www/server/nginx/sbin/nginx -V
```