# 其他

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