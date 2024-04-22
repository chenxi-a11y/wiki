# 给 VPS 配置 SSH 密钥免密登录

免密登录原理
利用密钥生成器制作一对密钥 —— 公钥和私钥。将公钥添加到服务器的某个账户上，然后在客户端利用私钥即可完成认证并登录。这样一来，没有私钥，任何人都无法通过 SSH 暴力破解你的密码来远程登录到系统。此外，如果将公钥复制到其他账户甚至主机，利用私钥也可以登录。

**1.生成 SSH 密钥对**
生成密钥可以在远端 VPS 上，也可以在本地终端上进行操作，方法都是类似的。

输入命令 

```
ssh-keygen


finalshell4.0以下需要生成PEM格式的私钥
生成时增加 -m PEM参数

ssh-keygen -m PEM -t rsa -C "注释"
```

 然后按 4 次 En­ter 键就行了。

root@p3ter:~# ssh-keygen  # 输入命令，按 Enter 键
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):   #  保存位置，默认就行，按 Enter 键
Enter passphrase (empty for no passphrase):   # 输入密钥密码，按 Enter 键。填写后每次都会要求输入密码，留空则实现无密码登录。
Enter same passphrase again:   # 再次输入密钥密码，按 Enter 键
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:GYT9YqBV4gDIgzTYEWFs3oGZjp8FWXArBObfhPlPzIk root@p3ter
The key's randomart image is:
+---[RSA 2048]----+
|*OO%+ .+o        |
|*=@.+++o.        |
| *o=.=....       |
|. +.B + +o.      |
| . + E *S.       |
|  o   o          |
|       .         |
|                 |
|                 |
+----[SHA256]-----+

现在，在当前用户的根目录中生成了一个 .ssh 的隐藏目录，内含两个密钥文件。id_rsa 为私钥，id_rsa.pub 为公钥。

**2.安装公钥**

密钥在远端
如果生成密钥的操作是在当前要添加的 VPS 上进行的，注意保存私钥 id_rsa 文件到本地。

然后输入以下命令安装公钥：

```
cd .ssh/
cat id_rsa.pub >> authorized_keys # 将公钥写入到 authorized_keys 文件
```

密钥在本地
如果生成密钥的操作是在本地终端上进行的，那么输入以下命令将公钥上传到 VPS：

```
ssh-copy-id user@remote -p port
user 为用户名，remote 为IP地址，port为端口号。
```

在没有 ssh-copy-id 的情况下（比如在 Win­dows 上）, 也可以用命令来操作：

```
ssh user@remote -p port 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```

命令的含义是，在远端新建 .ssh 文件夹，并把本地的 ~/.ssh/id_rsa.pub（公钥）追加到远端的 .ssh/authorized_keys 中。
当然，你也可以手动进行操作，先复制公钥，再登录 VPS ，粘贴到 .ssh/authorized_keys 中。



**3.修改权限**
为了确保连接成功，务必修改文件权限：

```
chmod 600 ~/.ssh/authorized_keys 或者 chmod 600 /root/.ssh/authorized_keys
chmod 700 ~/.ssh 修改主目录下.ssh目录的权限为700
 输入以下进行
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

**4.修改 sshd 配置文件**

开启密钥登录选项
编辑 /etc/ssh/sshd_config 文件找到以下参数进行更改：

```
RSAAuthentication yes
PubkeyAuthentication yes # yes表示允许密钥登陆
AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2 # 指定密钥的文件位置（如果不修改位置这一步不用修改）
```

重启 sshd 服务

```
service sshd restart
```

**5.禁用密码登录**
在你确认密钥能成功登录后，如果需要禁用密码登录，请更改以下参数：

```
PasswordAuthentication no
```

 不允许使用密码登陆，等测试密钥登陆成功了再修改此条，以防无法登陆

重启 sshd 服务

```
service sshd restart
```

至此，设置完成，不过我们需要保存好生成的私钥文件。

如果重启不了可用以下命令，没问题就不用

```
重启sshd服务，
Debian/Ubuntu执行 sudo /etc/init.d/ssh restart
CentOS执行：sudo /etc/init.d/sshd restart
注：centos7重启服务方式与以前不同，
请执行systemctl restart sshd.service
```



使用命令 userdel 删除用户账户
例：删除用户user2

userdel user2 
1
1
例：删除用户 user3，同时删除他的工作目录

userdel –r user3

userdel -r admin

userdel -r ubuntu


查看所有用户
直接查看/etc/passwd 文件后面第二个冒号的值大于1000时，这个就是一个用户


查看当前用户

1、shell终端中输入：who

当前用户为：book，使用tty7的终端，后面是登陆的时间



2、shell终端中输入：whoami

当前用户为：book，很精简输出结果



3、shell终端中输入：w

当前用户为：book，使用tty7的终端，后面是一些其他信息