# 宝塔反代+CDN后Nginx日志获取不到真实IP

将源站反代，同时套上了cloudflare后，发现源站Nginx网站日志，获取不到用户的真实IP，日志中记录到的都是反代服务器IP，或者是cloudflare的IP。

查看反代服务器Nginx日志，发现记录的也都是cloudflare的IP。

在源服务器和反代服务器Nginx配置http{ 段中，添加以下3行代码，问题顺利解决：

```
set_real_ip_from 0.0.0.0/0;
real_ip_header X-Forwarded-For;
real_ip_recursive on;
```

- set_real_ip_from：定义接受从哪个信任前代理处获得真实用户ip，可以定义多行，可定义为ip，ip段，支持ipv4和ipv6
- real_ip_header：定义从接收到报文的哪个http头部去获取前代理传送的用户ip，默认为X-Real-IP
- real_ip_recursive：递归搜索，默认为off

以下为引用：



```
    # 下面三行为重点，添加后就可以获取到客户端真实IP
    set_real_ip_from 0.0.0.0/0;
    real_ip_header  X-Forwarded-For;
    real_ip_recursive on;

    # 下面三行为常见反向代理传递真实客户端IP的配置，配置在http{}中，则全局应用在下面的所有server中
    proxy_set_header Host      $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

#### 获取真实IP

获取真实IP使用到的ngx_http_realip_module模块，会通过配置提取用户真实IP，并将其负值到$remote_addr，共有三个配置参数，分别是：

- set_real_ip_from：定义接受从哪个信任前代理处获得真实用户ip，可以定义多行，可定义为ip，ip段，支持ipv4和ipv6
- real_ip_header：定义从接收到报文的哪个http头部去获取前代理传送的用户ip，默认为X-Real-IP
- real_ip_recursive：递归搜索，默认为off

参数配置说明：

set_real_ip_from为定义信任ip，如果发送方的ip不在信任中，则不处理，$remote_addr就还会是发送发的地址。如果ip在信任中，则会根据下一个参数real_ip_header的设定，将指定的header头字段的数据作为$remote_addr。

如果real_ip_header中定义的header头字段，只传来了一个IP，比如默认的X-Real-Ip，则会直接将这个ip作为$remote_addr。这时候，如果像阿里云SLB，CDN，或者别的nginx等这类代理传来的，其真实IP可以是通过别的header字段传来的，那么跟去实际情况，定义此字段，这里我用的常见的X-Forwarded-For字段

这里需要说明一下X-Forwarded-For这个header信息，用于记录此请求所进过的ip，假设本nginx为第3层代理，那么获取到的X-Forwarded-For就会记录3个ip，分别顺序为：

```
用户IP 第一层代理IP 第二层代理IP
```

这时候就会用到real_ip_recursive参数，如果此参数不开启，就会从右往左，取第一个出现在信任中的IP的左边一位的IP作为$remote_addr，我们这里是全信任，所以就会取到第一层代理IP，这明显就并不一定对。如果开启了real_ip_recursive，那么就会从右边往左一直取到第一个不信任的IP作为$remote_addr，如果像我这里是全部信任，那么最左边的IP则会被作为$remote_addr。

#### 传递真实IP

在上面的配置中，使用proxy_set_header，通过设置header信息来传递真实IP，由于上面我们获取到了真实IP，然后可以通过默认的X-Real-IP头，将获取到的$remote_addr传递过去。也可以通过X-Forwarded-For 使用$proxy_add_x_forwarded_for在获取到的X-Forwarded-For末尾添加上自身的IP然后传递到下一层。设置Host为主机名，当发送发发来的header中有Host的header信息，则会作为$Host传递，如果没有则为nginx服务器主机名。这也是一种传递客户端信息的方式。

上面三行proxy_set_header为最常见的传递参数，注意由于很多时候header中不一定有Host字段，所以基本都是以IP作为客户端标识。