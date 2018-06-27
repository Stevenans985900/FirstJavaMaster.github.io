# 安装

参考 [全文搜索引擎 Elasticsearch 入门教程](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)

Elastic 需要 Java 8 环境. 可以跟着[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html)安装 Elastic.

解压后直接运行命令即可:
```
./bin/elasticsearch
```

## 常见错误

### WARN: unable to install syscall filter
```
[2016-11-06T16:27:21,712][WARN ][o.e.b.JNANatives ] unable to install syscall filter: 
java.lang.UnsupportedOperationException: seccomp unavailable: requires kernel 3.5### with CONFIG_SECCOMP and CONFIG_SECCOMP_FILTER compiled in
at org.elasticsearch.bootstrap.Seccomp.linuxImpl(Seccomp.java:349) ~[elasticsearch-5.0.0.jar:5.0.0]
at org.elasticsearch.bootstrap.Seccomp.init(Seccomp.java:630) ~[elasticsearch-5.0.0.jar:5.0.0]
```

解决：使用心得linux版本，就不会出现此类问题了。

### ERROR: bootstrap checks failed
```
max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536]
max number of threads [1024] for user [lishang] likely too low, increase to at least [2048]
```

解决：切换到root用户，编辑 /etc/security/limits.conf 添加类似如下内容
```
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
```

### ERROR: max number of threads
```
max number of threads [1024] for user [lish] likely too low, increase to at least [2048]
```

解决：切换到root用户，进入 /etc/security/limits.d/90-nproc.conf 目录下修改配置文件。
```
* soft nproc 1024
```
修改为
```
* soft nproc 2048
```

### ERROR: max virtual memory areas vm.max_map_count
```
max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```

解决：切换到root用户修改配置 /etc/sysctl.conf 添加下面配置：
```
vm.max_map_count=655360
```
并执行命令：
```
sysctl -p
```

### ERROR: max file descriptors
```
max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536]
```

解决：修改切换到root用户修改配置 /etc/security/limits.conf 添加下面两行
```
*        hard    nofile           65536
*        soft    nofile           65536
```
然后退出root.


**以上修改需要重新启动elasticsearch.如果未生效可尝试重新登录服务器**


## 后台运行

后台运行输入以下命令即可:
```
nohup ./bin/elasticsearch &
```

