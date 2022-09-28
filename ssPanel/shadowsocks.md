# shadowsocks-mod 一键安装脚本
这里提供了 SSPanel-UIM 官方配套后端的一键安装脚本使用说明。

!> 该脚本支持 RHEL 8-9，Fedora 34-36 使用 x86_64 或 aarch64 架构的系统。

**项目地址**
https://github.com/Anankke/shadowsocks-mod

**脚本功能**
- 配置 SSPanel-UIM RPM Repository
- 安装 shadowsocks-server
- 配置 shadowsocks-server
- 更新 SSPanel-UIM RPM Repository 和 shadowsocks-server
**安装**
```bash
dnf install wget -y
wget https://raw.githubusercontent.com/M1Screw/Airport-toolkit/master/ssr_node.sh
chmod +x ssr_node.sh
./ssr_node.sh install
```
**配置**
```bash
./ssr_node.sh config
```
**更新**
```bash
./ssr_node.sh update
```
**卸载**
```bash
./ssr_node.sh uninstall
```
**服务启动**
```bash
systemctl start shadowsocks-server
```
**服务停止**
```bash
systemctl stop shadowsocks-server
```
**注意事项**
- 所有跟节点安装本身无关的功能会通过单独的脚本提供（例如 BBR 加速一键配置功能在 Airport-toolkit 中通过 bbr_c7/bbr_c8.sh 脚本提供