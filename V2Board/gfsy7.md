# Telegram

V2Board内置Telegram功能介绍。

## 如何配置

**第一步**
Telegram联系@botfather创建机器人Token，创建后填入V2Board后台管理->Telegram->机器人Token

**第二步**
配置完成第一步后点击V2Board后台管理->Telegram->一键设置

**第三步**
V2Board后台管理->Telegram->开启机器人通知

## 支持指令

| 指令          | 参数     | 描述                                       |
| :------------ | :------- | :----------------------------------------- |
| /help         | 无       | 获取帮助信息，未匹配到指令时默认触发该指令 |
| /traffic      | 无       | 查看流量信息                               |
| /bind         | 订阅地址 | 绑定Telegram                               |
| /getlatesturl | 无       | 获取最新的站点地址                         |
| /unbind       | 无       | 解除绑定                                   |

## 通知

| 通知项目                    | 是否支持 |
| :-------------------------- | :------- |
| 管理员工单通知              | ☑️        |
| 客户支付通知                | ☑️        |
| forward工单通知进行快速回复 | ☑️        |

## 常见问题

Q：Bad Request: bad webhook: HTTPS url must be provided for webhook
A：不要在CDN或者其他中继层配置SSL，需要在部署网站的服务器配置SSL。

Q：发送消息给机器人没有得到答复？
A：首先先确保设置了Webhook，设置完成后如果没有收到请检查防火墙是否拦截了telegram的通知。