# 基本对接配置

1. 在`config.yml`中配置`PanelType: "SSpanel"`。

配置文件详见：[配置文件说明](https://xrayr-project.github.io/XrayR-doc/xrayr-pei-zhi-wen-jian-shuo-ming/config.html)

1. 对于sspanel >= 2021.11的版本中自动启用Custom_config的配置方法，请查看[SSPanel Custom Config](https://xrayr-project.github.io/XrayR-doc/dui-jie-sspanel/sspanel/sspanel_custom_config.html)，正确配置结点信息。关于订阅相关信息，请查看SSPanel相关文档：https://wiki.sspanel.org/#/universal-subscription。
2. 如果不想使用custom config，请在`ApiConfig`中将`DisableCustomConfig`设为`true`。同时参照[shadowsocks](https://xrayr-project.github.io/XrayR-doc/dui-jie-sspanel/sspanel/shadowsocks.html),[v2ray](https://xrayr-project.github.io/XrayR-doc/dui-jie-sspanel/sspanel/v2ray.html)和[trojan](https://xrayr-project.github.io/XrayR-doc/dui-jie-sspanel/sspanel/trojan.html)的配置方法，在sspanel地址栏中配置结点信息。