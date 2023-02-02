# Linux 下使用 Clash 科学上网

## 资源准备

**下载，clash主程序：** https://github.com/Dreamacro/clash/releases

**下载，dashboard：**https://github.com/Dreamacro/clash-dashboard

**下载，yacd：**https://github.com/haishanh/yacd

**使用Clash.Meta内核：**https://github.com/MetaCubeX/Clash.Meta

## 安装 Clash

1.下载当前操作系统与 CPU 架构对应的包文件，我这儿是 X86_64 平台下 CentOS7 所以对应的使用 clash-linux-amd64-v1.11.0.gz 包即可

```
#回到root目录
cd /root
#下载clash并重命名lash-linux-amd64-v1.11.0.gz为clash.gz
wget -O clash.gz https://github.com/Dreamacro/clash/releases/download/v1.11.0/clash-linux-amd64-v1.11.0.gz
#Meta内核版
wget -O clash.gz https://github.com/MetaCubeX/Clash.Meta/releases/download/v1.14.1/clash.meta-linux-amd64-v1.14.1.gz
#下载好后解压安装包中 clash 到 /usr/local/bin/ 目录下
gzip -dc clash.gz > /usr/local/bin/clash
#赋予执行权限
chmod +x /usr/local/bin/clash
#删除压缩包文件
rm -f clash.gz
#运行clash
/usr/local/bin/clash
```

在我们输入`./clash`运行clash时，clash会在当前用户主目录的`.config`目录下建立clash目录，并开始从github下载所需要的配置文件（主要是Country.mmdb），小白是以root用户运行的，也就是`/root/.config/clash`

2.创建 `systemd` 脚本，脚本文件路径为 `/etc/systemd/system/clash.service`，内容如下：

```
#进入插入编辑文本模式
vi /etc/systemd/system/clash.service

#退出和保存
#按【ESC】键跳到命令模式，按【i】键可以返回vi的输入模式编辑文件
#按【ESC】键跳到命令模式，然后再按【shfit】+【:】冒号键，最后再按【wq】，即可保存退出vi的编辑状态

#内容如下
[Unit]
Description=clash daemon

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/clash -d /root/.config/clash/
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

3.重载 systemctl daemon

```
#重新加载systemctl daemon
systemctl daemon-reload
```

## 配置代理上网

1.上传配置文件到/root/.config/clash/就行

```
从Windows版的clash中把yaml配置文件拷贝出来就好了：
配置→打开所在文件夹→并重命名为config.yaml
上传配置文件config.yaml到/root/.config/clash/就行
```

2.启动 `clash` 服务，并设置为开机自动启

```
#启动Clash
systemctl start clash
#或
systemctl start clash.service

#设置Clash开机自启动
systemctl enable clash
#或
systemctl enable clash.service


#以下为Clash相关的管理命令
#启动Clash
systemctl start clash.service
#停止Clash
systemctl stop clash.service
#重启Clash
systemctl restart clash.service
#查看Clash运行状态
systemctl status clash.service
```

3.设置系统代理，添加配置文件 `/etc/profile.d/proxy.sh` 并在其中写入如下内容：

```
#进入插入编辑文本模式
vi /etc/profile.d/proxy.sh
#退出和保存
#按【ESC】键跳到命令模式，按【i】键可以返回vi的输入模式编辑文件
#按【ESC】键跳到命令模式，然后再按【shfit】+【:】冒号键，最后再按【wq】，即可保存退出vi的编辑状态

#内容如下
export http_proxy="127.0.0.1:7890"
export https_proxy="127.0.0.1:7890"
export no_proxy="localhost, 127.0.0.1"
```

4.重载 `/etc/profile` 配置

```
source /etc/profile
```

```
#调用http代理
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
#测试
curl ip.sb
#如果看到返回的IP是机场节点的IP，那说明clash运行正常
#取消代理
unset http_proxy
```



## 配置 web-ui

如果提示未找到git命令那就yum一下：yum install git -y

1. 克隆 [clash-dashboard](https://github.com/Dreamacro/clash-dashboard) 项目到本地

   ```
   git clone -b gh-pages --depth 1 https://github.com/Dreamacro/clash-dashboard /opt/clash-dashboard
   ```

2. 修改 `clash` 配置文件中 `external-ui` 的值为 `/opt/clash-dashboard`

   ```
   sed -i "s/^#\{0,1\} \{0,1\}external-ui.*/external-ui: \/opt\/clash-dashboard/" /root/.config/clash/config.yaml
   ```

3. 重启 clash 服务

   ```
   systemctl restart clash
   ```

4. 通过浏览器访问 `localhost:9090/ui`，其中 `localhost` 替换为 clash 部署服务器的 IP