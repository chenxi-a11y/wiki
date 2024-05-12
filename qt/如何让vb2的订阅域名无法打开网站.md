# 如何让vb2的订阅链接输入域名无法打开网站，但是添加上后面目录又能获取订阅信息？

问下各位如何让vb2的订阅链接输入域名无法打开网站，但是添加上后面目录又能获取订阅信息？

利用nginx 限制

```shell
location = /api/v1/client/subscribe {
        proxy_ssl_name jisu-mb.top;
        proxy_ssl_server_name on;
        proxy_pass https://jisu-mb.top;
        proxy_set_header Host jisu-mb.top;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header Upgrade $http_upgrade;
        #proxy_hide_header Upgrade;
        proxy_http_version 1.1;
        #Persistent connection related configuration
        add_header X-Cache $upstream_cache_status;

        proxy_ignore_headers Set-Cookie Cache-Control expires;
        add_header Cache-Control no-cache;
        expires 12h;
 }
location / {
      deny all;
      return 403;
}
```