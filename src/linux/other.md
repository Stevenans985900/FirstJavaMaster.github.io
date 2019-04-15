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

踢掉某用户
`pkill -kill -t pts/1`

## 控制root用户的ssh登录

编辑文件

`sudo vi /etc/ssh/sshd_config`

`PermitRootLogin`这个参数，它的值可以为`yes/no/without-password`

```
yes #允许root用户以任何认证方式登录（貌似也就两种认证方式：用户名密码认证，公钥认证）

without-password #只允许root用public key认证方式登录

no #不允许root用户以任何认证方式登录
```
