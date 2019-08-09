# 其他

## 开机启动

### centos 6

使用chkconfig命令即可。我们以apache服务为例：

`chkconfig --add apache` 添加nginx服务

`chkconfig apache on` 开机自启nginx服务

`chkconfig apache off` 关闭开机自启

`chkconfig --list | grep apache` 查看

### centos 7
使用systemctl中的enable、disable 即可。示例：

`systemctl enable apache.service` 开机自启apache服务

`systemctl disable apache.service` 关闭开机自启


## 系统

+ 查看网络连接
`ip addr`

+ 查看内核信息
`uname -a`

+ 查看版本信息
`cat /etc/os-release`
`head -1 /etc/issue`

+ 查看内核
`yum install lsb –y`
`lsb_release -a`

+ vim tab to 4 space

编辑/etc/vimrc文件,添加下面的内容

```
" vim tab to 4 space
set ts=4
set expandtab
set autoindent
```

+ ssh 代理

```
ssh -o ProxyCommand='nc -x 127.0.0.1:{socks_port} {server_ip} 22' user@server_ip
```

+ 探测TCP端口

    telnet 192.168.80.131 8888

+ 探测UDP端口

    nc -vuz 112.91.151.10 4500


