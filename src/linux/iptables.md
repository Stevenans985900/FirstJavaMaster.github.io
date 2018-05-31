# iptables

iptables是CentOS7之前常用的系统防火墙工具.

配置文件位于 `/etc/sysconfig/iptables`,修改此文件后重启iptables软件即可使规则生效.


## 基本操作

+ 查看防火墙状态 `service iptables status`
 
+ 停止防火墙 `service iptables stop`
 
+ 启动防火墙 `service iptables start`
 
+ 重启防火墙 `service iptables restart`
 
+ 永久关闭防火墙 `chkconfig iptables off`
 
+ 永久关闭后重启 `chkconfig iptables on`

