# 进阶

## 前端分离

V2Board的前端文件可以从V2Board分离出来进行部署，这是V2Board的一点特色，可以提升用户的访问体验。

前端目前分为2端

- [用户](https://github.com/v2board/v2board-user/releases)
- [管理员](https://github.com/v2board/v2board-admin/releases)

你可以选择对应的V2Board版本对这两个前端分别进行部署

部署方式非常简单，你只需要下载或者克隆到HTML站点并配置`env.example.js`并重命名为`env.js`即可完成

## Clash配置文件自定义

首先你需要前往 **/resources/rules** ，这个目录下存放着订阅软件相关配置文件

复制一份`default.clash.yaml`重命名为`custom.clash.yaml`并且配置成自己想要的形式，其中`$app_name`参数为站点名称可以参考默认配置文件配置。保存后将会优先使用`custom.clash.yaml`，需要恢复默认配置删除即可。

后续V2board进行更新则不会覆盖该配置。

参考下列示例可以对Proxies进行过滤

```
proxy-groups:
  - { name: "$app_name", type: select, proxies: ["自动选择", "故障转移"] }
  - { name: "自动选择", type: url-test, proxies: [], url: "http://www.gstatic.com/generate_204", interval: 86400 }
  - { name: "故障转移", type: fallback, proxies: [], url: "http://www.gstatic.com/generate_204", interval: 7200 }
  // 使用正则将节点名称包含香港的节点分配到这个规则中
  - { name: "香港", type: select, proxies: [/香港/, "自动选择"] }
```

## 客户端的适配

市面上形形色色的客户端均可以通过参考 `app/Http/Controllers/Client/Protocols/` 目录下文件进行配置。

## 常见问题

Q：前后端分离后登陆后立马退出是怎么回事？
A：前段使用的域名跟后端使用的域名必须是同一个根域。