# [Linux VPS通过安装CPULimit来限制CPU使用率](https://blog.shmily.one/linux/linux-vps-cpulimit.html)

香港教程：

[linux宝塔面板环境安装cpulimit 解决cpu使用率过高 - rodney的火星 – 发现、品味、记录、分享精彩与快乐的生活｜新知](https://rodney.im/share_1265.html)

[linux遇到的坑以及总结的经验！ - 南山如雪 (ckajx.com)](https://www.ckajx.com/archives/xuexi)

[linux宝塔面板环境安装cpulimit 解决cpu使用率过高 - 大鸟博客 (daniao.org)](https://www.daniao.org/12979.html)

[Linux VPS通过安装CPULimit来限制CPU使用率-Shmily's Blog](https://blog.shmily.one/linux/linux-vps-cpulimit.html)

[搬瓦工 VPS 使用 CPULimit 来限制 Linux 进程的 CPU 使用率教程 - Bandwagonhost中文网-Bandwagonhost中文网](https://www.bandwagonhost.net/10685.html)

[Linux VPS服务器通过安装CPULimit来限制CPU使用率or占用率_恒创科技 (henghost.com)](https://m.henghost.com/news/article/57799/)

#### 1、安装cpulimit 

安装自己用包安装即可。

**1）Centos：**

```markdown
yum install cpulimit 
```

**2）Debian/Ubuntu**

```csharp
apt-get install cpulimit
```

**3）如果没有包，也不能编译安装，可以安装EPEL源，如下：**

隐藏的内容

```markdown
#安装EPEL源 
yum -y install epel-release 
			
#重新创建本地仓库缓存 
yum clean all && yum makecache 
			
#然后yum下就可以了 
yum install cpulimit	
```





## 说明

```
cpulimit -h
Usage: cpulimit [OPTIONS...] TARGET
   OPTIONS
      -l, --limit=N          percentage of cpu allowed from 0 to 100 (required)//cpu限制的百分比
      -v, --verbose          show control statistics//显示版本号
      -z, --lazy             exit if there is no target process, or if it dies//如果限制的进程不存在了，则退出。
      -i, --include-children limit also the children processes//包括子进程。
      -h, --help             display this help and exit //帮助，显示参数
   TARGET must be exactly one of these:
      -p, --pid=N            pid of the process (implies -z) //进程的pid
      -e, --exe=FILE         name of the executable program file or path name //可执行程序
      COMMAND [ARGS]         run this command and limit it (implies -z)
```

 

## 用法

```
限制firefox使用30% cpu 利用率
cpulimit -e firefox -l 30

限制进程号1313的程序使用30% cpu 利用率
cpulimit -p 1313 -l 30

限制绝对路径下该软件的 cpu 利用率
cpulimit -e /usr/local/nginx/sbin/nginx -l 50

最后，如果想要停止刚刚已经限制的进程，那么需要通过 top 查找 PID 然后 kill：
kill -9 PID

CPULimit 进阶用法
1、后台运行
cpulimit 在执行时也会占用一个终端机，若想让 cpulimit 在后台运行，可加上 --background 参数：
cpulimit --pid 21203 --limit 50 --background

2、终止 CPU 用量过高的进程
cpulimit 配合 --limit 参数可以限制进程的 CPU 用量上限值，如果进程超过这个上限值，预设会调节 CPU 用量，而如果想要在 CPU 用量过高时直接中止进程，可以加上 –-kill 参数：
cpulimit --pid 21203 --limit 50 --kill
```

 

## 注意

- -l后面限制的cpu使用量，要根据实际的核心数量而成倍减少。40%的限制生效在1核服务器中，如果是双核服务器，则应该限制到20%，四核服务器限制到10%以此类推。

## 限制所有进程的 CPU 使用率

```
#!/bin/bash 

while true ; do

  id=`ps -ef | grep cpulimit | grep -v "grep" | awk '{print $10}' | tail -1`

  nid=`ps aux | awk '{ if ( $3 > 75 ) print $2 }' | head -1`

  if [ "${nid}" != "" ] && [ "${nid}" != "${id}" ] ; then

    cpulimit -p ${nid} -l 75 &

    echo "[`date`] CpuLimiter run for ${nid} `ps -ef | grep ${nid} | awk '{print $8}' | head -1`" >> /root/cpulimit-log.log

  fi

  sleep 3

done
```

 

默认情况下 cpulimit 只能对已经存在的进程进行限制，但是设置此脚本为随机自启动即可(设置方法参看上面的脚本链接中)，它会对所有进程（包括新建进程）进行监控并限制（3秒检测一次，CPU限制为75％）

保存到 `/root/cpulimit.sh`，会自动生成日志文件 `/root/cpulimit-log.log`

然后将此降本添加开机启动。

## 设置为开机启动

修改 `/etc/rc.local` 在对应位置加入 `/root/cpulimit.sh` 再重启系统，就会全程限制各个进程的 CPU 使用了！

## 注意

在这里我是使用screen命令来保持脚本的后台运行的

## 补充

在网上搜到了另外一种解决方案

1.在/root/下建立cpulimit.sh 给执行权限（放开占用率10%以下的程序，限制60%以上的程序）

```
cpulimit --pid `ps aux|awk '{if($3 < 10) print $2}'` --limit=99
cpulimit --pid `ps aux|awk '{if($3 > 60) print $2}'` --limit=25
```

 

2.安装cpulimt，

```
apt-get install cpulimit -y
```

 

3.把执行cpulimt.sh写入crontab，每隔五分钟执行一次，如果有任何程序cpu使用超过60%，就限制到25%，同时放开占用低的程序的限制。

但是crontab暂时买看懂怎么写…遂采用的第一种方案