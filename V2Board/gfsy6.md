# 知识库

V2board内置的知识库功能，配合自带变量和展现形式方便你对教程进行管理，用户更容易上手。

## 基本

你可以在知识库编辑器使用 **HTML** 或 **Markdown** 进行写作。

跳转函数：`jump(知识库文章ID)`
复制函数：`copy(string)`
订阅地址：`{{subscribeUrl}}`
订阅地址：`{{urlEncodeSubscribeUrl}}`
订阅地址：`{{safeBase64SubscribeUrl}}`
站点名称：`{{siteName}}`

## 仅为有效用户显示

你可以在文档编辑器中使用以下语法，可以仅对订阅处于有效的用户展示区域内的内容。

```
<!--access start-->
展示内容...
<!--access end-->
```

## 最佳实践

1.你可以结合订阅地址在知识库中写出任意客户端类型的意见订阅，比如在订阅地址后边加上客户端flag

```
{{subscribeUrl}}&flag=v2rayng
```

2.你可以在自定义订阅有效内容中加上任何类型的账号密码，他们将只会被有效用户看到。