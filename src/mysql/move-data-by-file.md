# 通过mysql的data目录下文件迁移数据

因为前段时间调整服务器分区时不小心,导致其中一个分区无法挂载,mysql也无法启动.mysql里还有很多数据需要导出,只好通过一些**特殊方法**导出数据.

MySQL里存储的数据都存放在一个data目录下,默认是 /var/lib/mysql 目录.具体的路径要查看其配置文件 /etc/my.cnf 中**datadir**一项的值.


下面介绍如何利用此目录下的文件,将数据库中的数据从A服务器迁移到B服务器.

1. 保证B服务器安装的MySQL软件版本和A服务器**相同或相近**(没有测试跨大版本如5.6->5.7的情况).
2. 保证B服务器mysql可以正常运行.
3. 关闭B服务器的mysql.
4. 删掉B服务器上mysql的data目录下的东西(最好对其**备份**).
5. 将A服务器data目录下的文件拷贝到B服务器data目录.保证文件**所属**与B服务器原文件相同(一般是属于mysql:mysql)
6. 修改my.cnf,加入**innodb_force_recovery=4**或者**6**
7. 启动B服务器的MySQL,注意err文件的报错(err文件路径即 my.cnf 文件中**log-error**一项的值).可能遇到权限错误之类的,一个一个调整即可.
8. 启动B服务器的MySQL后可尝试登录(MySQL的用户名和密码用原来A服务器的账号或B服务器的账号),查询其中的数据.
9. 或者直接进行mysqldump,将数据导出到sql文件.SQL语句为: `mysqldump -uroot -ppassword -A > mysql_dump.sql`
10. 至此数据恢复成功!


个人实践来看,最好是B服务器的MySQL是全新安装的.或者直接上docker,这样环境比较干净.

而且貌似此种方法只能恢复InnoDB引擎的数据,其他引擎据说有问题,未经过测试.

参考:

1. [https://bbs.csdn.net/topics/390966560](https://bbs.csdn.net/topics/390966560)
2. [https://bbs.csdn.net/topics/390933416](https://bbs.csdn.net/topics/390933416)
3. [https://www.cnblogs.com/xxx91hx/p/6425438.html](https://www.cnblogs.com/xxx91hx/p/6425438.html)