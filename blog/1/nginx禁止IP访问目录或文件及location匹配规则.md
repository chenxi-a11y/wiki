# nginx禁止IP访问目录或文件及location匹配规则

### 一、关于deny和allow的使用

nginx通过ngx_http_access_module模块来控制IP访问特定路径，可以放在`http`, `server`, `location`中。比如：

```
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
}
```

nginx按顺序检查规则，直到找到第一个匹配项。注意在使用指令时，如果最后不添加deny all,则可能会允许上面列出ip之外的其他ip均可访问，因为默认是allow all的。

```
   deny 43.243.12.116;
   deny 43.241.242.243;
   allow all;
```

所以，上面例子中最后的allow all可以不用添加，因为默认是allow all的。

### 二、nginx location 配置

1. 禁止访问某些后缀文件

```
location ~ \.(ini|conf|txt)$ {
    deny all;
}
```

2. 禁止访问目录或目录下文件

```
#禁止访问目录
location ^~ /test/ {
    deny all;
}
#禁止访问目录下文件
location ^~ /test {
    deny all;
}
```

3. 禁止访问某个目录下的指定文件后缀文件

```
# 禁止访问某个目录下的 php 后缀文件
location /directory {
    location ~ .*\.(php)?$ {
    deny all;
    }
}
# 禁止访问多个目录下的 php 后缀文件
location ~* ^/(directory1|directory2)/.*\.(php)${
    deny all;
}
```

location可以嵌套使用，也就是说一个location中可以继续嵌套location。

### 三、location匹配规则详解

= 表示精确匹配

^~ 表示uri以某个字符串开头

~ 正则匹配(区分大小写)

~* 正则匹配(不区分大小写) !~和!~*分别为区分大小写不匹配及不区分大小写不匹配的正则

/ 任何请求都会匹配

匹配优先级： = > ^~ > /

举例：

```
    location ~ .*\.(json)?$ {
    allow 127.0.0.1/16;
    deny all;
    expires      -1;
    }
```

1. `location`: 表示定义一个匹配规则的位置块。
2. `~`: 表示后面跟着的是一个正则表达式，而不是普通的字符串匹配。
3. `.*`: 表示匹配任意字符（除了换行符）零次或多次。这里是匹配任意字符零次或多次，即允许路径中包含任意字符。
4. `\.`: 表示匹配点（.）字符。在正则表达式中，点（.）通常表示任意字符，但在这里通过反斜杠进行转义，表示匹配实际的点字符。
5. `(json)?`: 表示匹配括号内的内容零次或一次，即json可选。这里的括号用于创建一个捕获组，表示括号内的内容是一个整体，而问号表示前面的字符（这里是 `(json)`）可选。
6. `$`: 表示匹配字符串的结尾。

参考文档：



https://nginx.org/en/docs/http/ngx_http_access_module.html

https://blog.51cto.com/niuben/5826635