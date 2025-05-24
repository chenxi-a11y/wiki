# 宝塔Nginx反向代理Cloudflare(CF)的https站点

## 自己的配置

### 无缓存配置

```
#PROXY-START/
location  ~* \.(php|jsp|cgi|asp|aspx)$
{
    proxy_pass https://要反代的域名;
    proxy_set_header Host 要反代的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 要反代的域名;
    proxy_ssl_server_name on;
 
}
location /
{
    proxy_pass https://要反代的域名;
    proxy_set_header Host 要反代的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 要反代的域名;
    proxy_ssl_server_name on;
 
 
    #缓存设置
    add_header X-Cache $upstream_cache_status;
    
    #Set Nginx Cache
    
    	add_header Cache-Control no-cache;
}
 
#PROXY-END/
 
```

 

### 无缓存且进行内容替换

```
#PROXY-START/
location  ~* \.(php|jsp|cgi|asp|aspx)$
{
    proxy_pass https://要反代的域名;
    proxy_set_header Host 要反代的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 要反代的域名;
    proxy_ssl_server_name on;
 
}
location /
{
    proxy_pass https://要反代的域名;
    proxy_set_header Host 要反代的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 要反代的域名;
    proxy_ssl_server_name on;
 
 
    #缓存设置
    add_header X-Cache $upstream_cache_status;
    
    #Set Nginx Cache
 
    proxy_set_header Accept-Encoding "";
	sub_filter "被替换内容" "替换内容";
    sub_filter_once off;
    	add_header Cache-Control no-cache;
}
 
#PROXY-END/
 
```

 

## 以下为作者博客原文

## 自己域名反代自己在 cloudflare 的域名进行加速

```
#PROXY-START/
location  ~* \.(php|jsp|cgi|asp|aspx)$
{
    proxy_pass https://你的域名;
    proxy_set_header Host 你的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 你的域名;
    proxy_ssl_server_name on;
 
}
location /
{
    proxy_pass https://你的域名;
    proxy_set_header Host 你的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 你的域名;
    proxy_ssl_server_name on;
 
 
    #缓存设置
    add_header X-Cache $upstream_cache_status;
        #Set Nginx Cache
 
    proxy_ignore_headers Set-Cookie Cache-Control expires;
    proxy_cache cache_one;
    proxy_cache_key $host$uri$is_args$args;
    proxy_cache_valid 200 304 301 302 120m;
    expires 12h;
}
 
#PROXY-END/
```

## 自己的域名反代别人在 cloudflare 的域名

```
#PROXY-START/
location  ~* \.(php|jsp|cgi|asp|aspx)$
{
    proxy_pass https://对方的域名;
    proxy_set_header Host 对方的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 对方的域名;
    proxy_ssl_server_name on;
 
}
location /
{
    proxy_pass https://对方的域名;
    proxy_set_header Host 对方的域名;
    #向后端传递访客 ip
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    #向后端传递访客 ip
   
    proxy_ssl_name 对方的域名;
    proxy_ssl_server_name on;
 
 
    #缓存设置
    add_header X-Cache $upstream_cache_status;
        #Set Nginx Cache
 
    proxy_ignore_headers Set-Cookie Cache-Control expires;
    proxy_cache cache_one;
    proxy_cache_key $host$uri$is_args$args;
    proxy_cache_valid 200 304 301 302 120m;
    expires 12h;
}
 
#PROXY-END/
```

## 反代 cloudflare 的站点下 nginx 的缓存和[反代](https://blog.kieng.cn/tag/反代)的缓存设置

1.如果源站设置 expires、源站端 max-age 和[反代](https://blog.kieng.cn/tag/反代)nginx cahe 端的 proxy_cache_valid 的情况下,最终是以源站设置的 expires 的值进行缓存过期处理

2.假如在[反代](https://blog.kieng.cn/tag/反代)nginx 中设置了相关配置，取消源站 expires 对缓存的影响(proxy_ignore_headers)，在同时设置了源站 expires、源站端 max-age 和[反代](https://blog.kieng.cn/tag/反代)nginx cache 端的 proxy_cache_valid 的情况下，最终以源站端 max-age 的值进行缓存过期处理

3.假如取消源站 expires 和源站端 max-age 对缓存的影响，则以[反代](https://blog.kieng.cn/tag/反代)nginx 端 proxy_cache_valid 设置的值为标准进行缓存的过期处理

4.[反代](https://blog.kieng.cn/tag/反代)nginx 端 inactive 的值不受上面所影响，就是请求页面后，根据 inactvie 设置的时间，都会强制进行缓存清理

5.所以对缓存过期的优先级进行排序为：inactvie、源站 expires、源站端 max-age、反代 nginx 的 proxy_cache_valid

```
#以下配置放在 http 区,不要在 server 区使用
proxy_temp_path /www/server/nginx/proxy_temp_dir;
proxy_cache_path /www/server/nginx/proxy_cache_dir levels=1:2 keys_zone=cache_one:20m inactive=1d max_size=5g;
#这一段就是反代的缓存基本设置
#proxy_cache_path:缓存数据目录
#levels:按照几层目录分级
#keys_zone:key 空间名,后面的大小为 key 空间大小,1m 可以存放 8000 左右 key,所以不用设得过大
#inactive:强制更新时间，在指定时间内没人访问，就删除缓存(这个很重要,见上面说明,如果不会手动设置 proxy_cache_valid,或者懒得设置的话,把这个值设置为 1m,就会达到缓存内容 1 分钟没人访问自动清理)
#max_size:这个才是缓存数据的大小限制
client_body_buffer_size 512k;
#缓冲区代理缓冲用户端请求的最大字节数
proxy_connect_timeout 60;
#nginx 跟后端服务器连接超时时间(代理连接超时)
proxy_read_timeout 60;
#连接成功后，后端服务器响应时间(代理接收超时)
proxy_send_timeout 60;
#请求的超时时间
proxy_buffer_size 32k;
#设置代理服务器（nginx）保存用户头信息的缓冲区大小
proxy_buffers 4 64k;
#proxy_buffers 缓冲区，网页平均在 64k 以下
proxy_busy_buffers_size 128k;
#高负荷下缓冲大小（建议值为 proxy_buffers*2）
proxy_temp_file_write_size 128k;
proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
proxy_cache cache_one;
#为缓存区名字
```

```
#以下缓存配置在反代规则里需要缓存的路径(不需要缓存的路径或者规则不要添加下面的)
add_header X-Cache $upstream_cache_status;
#增加头信息,可以在浏览器 F12 里观察
proxy_ignore_headers Set-Cookie Cache-Control expires;
#重要,强制缓存,不然有些页面不缓存
proxy_cache cache_one;
#开启缓存 缓存区名称
proxy_cache_key $host$uri$is_args$args;
#缓存 key
proxy_cache_valid 200 304 2h;
proxy_cache_valid 301 302 3d;
proxy_cache_valid any 10m;
#状态码 200,304 状态缓存 2 小时-301,302 的过期为 3 天,其余状态码 10 分钟过期
```

这样配合上面的话就是，常用的缓存文件，比如 200 304 状态的缓存为 2 小时，301 302 状态的缓存为三天，其余状态 10 分钟
这样不用手动去清理缓存文件，自动的清理了。