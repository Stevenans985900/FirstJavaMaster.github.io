# 使用lsyncd实现跨服务器文件同步

lsyncd是linux上一款文件同步的软件. 它基于rsync, 不过使用更加简单方便.

rsync默认已经安装在centOS系统中.

## 安装lsyncd

可直接通过yum进行安装

```bash
yum install lsyncd
```

安装完成后,其配置文件位于`/etc/lsyncd.conf`


## 进行配置

```text
----
-- User configuration file for lsyncd.
--
-- Simple example for default rsync, but executing moves through on the target.
--
-- For more examples, see /usr/share/doc/lsyncd*/examples/
--
--sync{default.rsyncssh, source="/var/www/html", host="localhost", targetdir="/tmp/htmlcopy/"}

settings {
    logfile = "/var/logs/lsyncd.log",
    maxDelays = 5
}

sync {
    default.rsyncssh,
    source = "/data/backups/localhost",
    host = "192.168.2.52",
    targetdir = "/data/backups/192.168.2.51",
    maxDelays = 5,
    delay = 30,
    rsync = {
        archive = true,
        compress = true,
        bwlimit   = 2000
    }
}
```

### 上面配置项的解释

#### settings

里面是全局设置,下面是几个常用选项说明:
+ `logfile` 定义日志文件
+ `stausFile` 定义状态文件
+ `nodaemon=true` 表示不启用守护模式，默认
+ `statusInterval` 将lsyncd的状态写入上面的statusFile的间隔，默认10秒
+ `inotifyMode` 指定inotify监控的事件，默认是CloseWrite，还可以是Modify或CloseWrite or Modify
+ `maxProcesses` 同步进程的最大个数。假如同时有20个文件需要同步，而maxProcesses = 8，则最大能看到有8个rysnc进程
+ `maxDelays` 累计到多少所监控的事件激活一次同步，即使后面的delay延迟时间还未到

#### sync

里面是定义同步参数,可以继续使用`maxDelays`来重写settings的全局变量。

一般第一个参数指定lsyncd以什么模式运行:
+ `default.rsync` 本地目录间同步，使用rsync，也可以达到使用ssh形式的远程rsync效果，或daemon方式连接远程rsyncd进程；
+ `default.direct` 本地目录间同步，使用cp、rm等命令完成差异文件备份；
+ `default.rsyncssh` 同步到远程主机目录，rsync的ssh模式，需要使用key来认证

其他配置项:

+ `source` 同步的源目录，使用绝对路径。
+ `host` 远程服务器
+ `targetdir` 定义目的地
+ `init` 这是一个优化选项，当值为false时，只同步进程启动以后发生改动事件的文件，原有的目录即使有差异也不会同步。默认是true
+ `delay` 累计事件，等待rsync同步延时时间，默认15秒（最大累计到1000个不可合并的事件）。也就是15s内监控目录下发生的改动，会累积到一次rsync同步，避免过于频繁的同步。（可合并的意思是，15s内两次修改了同一文件，最后只同步最新的文件）
+ `excludeFrom` 排除选项
```
后面指定排除的列表文件，如excludeFrom = "/etc/lsyncd.exclude"，如果是简单的排除，可以使用exclude = LIST。

这里的排除规则写法与原生rsync有点不同，更为简单：
监控路径里的任何部分匹配到一个文本，都会被排除，例如

/bin/foo/bar可以匹配规则foo
如果规则以斜线/开头，则从头开始要匹配全部
如果规则以/结尾，则要匹配监控路径的末尾
?匹配任何字符，但不包括/
*匹配0或多个字符，但不包括/
**匹配0或多个字符，可以是/
```

+ `delete` 为了保持target与souce完全同步，Lsyncd默认会delete = true来允许同步删除。它除了false，还有startup、running值

#### rsync

+ `bwlimit` 限速，单位kb/s，与rsync相同
+ `compress` 压缩传输默认为true。在带宽与cpu负载之间权衡，本地目录同步可以考虑把它设为false
+ `perms` 默认保留文件权限。
+ 其它rsync的选项


### 启动lsyncd

#### centOS6

`sudo service lsyncd start` 启动服务

`sudo chkconfig lsyncd on` 开机自启动

#### centOS7

`sudo systemctl start lsyncd` 启动服务

`sudo systemctl status lsyncd` 查看服务启动是否成功

`sudo systemctl enable lsyncd` 开机自启动


### 其他注意事项

以ssh方式同步需要的配置最少,个人推荐这种.不过默认是以root(确切的说是当前启动lsyncd的用户)来执行命令.所以需要打通root账号的ssh免密登录.

生成公钥密钥的命令: `ssh-keygen -t rsa`

追加到目标服务器上`/root/.ssh/authorized_keys`文件中.


#### 更多信息

[GitHub](https://github.com/axkibe/lsyncd)

[Doc](https://axkibe.github.io/lsyncd/)

[lsyncd实时同步搭建指南——取代rsync+inotify | Sean's Notes](http://seanlook.com/2015/05/06/lsyncd-synchronize-realtime/)