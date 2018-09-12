## 更换服务器的mysql数据存放目录

使用yum安装在CentOS的mysql程序默认将其数据文件存放在 `/var/lib/mysql` 目录下.
但是很多时候我们不会给此目录进行单独分区, 所以有些时候此分区的硬盘空间就不够用了. 我们需要自定义它的数据文件存放目录.

这里我们设定新的目录为 `/data/lib` 文件夹下. **移动前最好对数据库的数据进行备份**

1. 停止mysql服务

    CentOS 6 系统执行 `sudo service mysqld stop`
    
    CentOS 7 系统执行 `sudo systemctl stop mysqld`

2. 移动原有的数据目录到新目录
   ```
   cd /data
   mkdir lib
   mv /var/lib/mysql /data/lib
   ```

3. 修改配置文件
   
    mysql的配置文件默认是 `/etc/my.conf`. 修改其中的内容:
    ```
    # datadir=/var/lib/mysql
    # socket=/var/lib/mysql/mysql.sock
    datadir=/data/lib/mysql
    socket=/data/lib/mysql/mysql.sock
    ```

    可选内容(如果后面启动失败或登录失败,可继续配置以下内容):
    ```
    [mysql.server]
    user=mysql
    basedir=/data/lib/mysql
    [client]
    socket=/var/lib/mysql/mysql.sock
    ```

4. 启动数据库, 查看是否正常

    