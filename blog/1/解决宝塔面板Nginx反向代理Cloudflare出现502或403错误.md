# 解决宝塔面板Nginx反向代理Cloudflare出现502或403错误

解决宝塔面板Nginx反向代理Cloudflare出现502或403错误
首先源站配置好SSL证书，然后去Cloudflare开启SSL（默认开启）

然后宝塔面板 – 站点设置 – 反向代理 – 配置文件，在

```
location /
{
proxy_pass https://www.domain.com;
proxy_set_header Host www.domain.com;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header REMOTE-HOST $remote_addr;

代码下面，新增

proxy_ssl_server_name on;
```

如果修改后还提示502错误

在

```
location /
{
proxy_pass https://www.domain.com;
proxy_set_header Host www.domain.com;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header REMOTE-HOST $remote_addr;

代码下面，新增

proxy_ssl_name 被反向代理的域名;

proxy_ssl_server_name on;
```

