## 用户和权限

+ 创建用户

    CREATE USER 'slkj'@'%' IDENTIFIED BY 'idx365';

+ 修改用户密码

    ALTER USER 'slkj'@'%' IDENTIFIED BY 'MyNewPass4!';

+ 删除用户

    DROP USER 'slkj'@'host';

+ 查看所有用户

    select host, user from user;

+ 更新ip绑定

    update user set host = 'localhost' where user = 'slkj' and host = '%';

+ 查看权限

    show grants for 'slkj';

+ 给用户 slkj 赋予所有库的所有权限
  + MySQL7.X及以下

    GRANT ALL PRIVILEGES ON *.* TO 'slkj'@'%' IDENTIFIED BY 'idx365' WITH GRANT OPTION;

  + MySQL8.X

    GRANT ALL PRIVILEGES ON *.* TO 'slkj'@'%';

+ 重新载入赋权表  

    FLUSH PRIVILEGES;
  
+ 收回权限(不包含赋权权限)

    REVOKE ALL PRIVILEGES ON slkj.* FROM slkj;

+ 收回赋权权限  

    REVOKE GRANT OPTION ON *.* FROM slkj;


### MySQL 8 的密码加密

> SQLyog需要使用'mysql_native_password'身份验证方法进行连接。 Oracle引入了“caching_sha2_password”，并在MySQL 8.0中将其设置为默认值。 但是这个新的身份验证方法在客户端代码中还没有插件支持，我们在SQLyog（MariaDB的“Connector/C”）中使用。 实际上，只有来自Oracle的客户端和连接器才能使用新的认证插件。
> 
> 所以,在mysql 8.0中,涉及到用户的密码添加/修改操作时,需要将类似
> 
> `ALTER USER 'slkj'@'%' IDENTIFIED BY 'MyNewPass4!';`
> 
> 的语句改为
> 
> `ALTER USER 'slkj'@'%' IDENTIFIED WITH mysql_native_password BY 'mypass';`


### MySQL 8 的密码强度验证

MySQL 8 的密码安全度验证规则修改和 MySQL 7 不太一致。

查看已有的验证规则：

    SHOW VARIABLES LIKE 'validate_password%';

设置变量（验证规则为‘LOW’，长度至少6位）：

    set global validate_password.policy=0;
    set global validate_password.length=6;

然后退出 MySQL 执行以下命令：

    mysql_secure_installation

会有很多提示，比如是否重置root用户密码等。要注意判断。