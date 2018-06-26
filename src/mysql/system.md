# 系统部分

## 数据库结构查询

+ 查询表格字段设计

```mysql
select
  TABLE_NAME,
  COLUMN_NAME,
  COLUMN_DEFAULT,
  IS_NULLABLE,
  COLUMN_TYPE,
  CHARACTER_MAXIMUM_LENGTH,
  COLUMN_COMMENT
from information_schema.COLUMNS
where TABLE_SCHEMA = 'index_center';
```

+ 查询表的索引信息

```
select
  TABLE_NAME,
  INDEX_NAME,
  COLUMN_NAME
from information_schema.STATISTICS
where TABLE_SCHEMA = 'index_center';
```

## 系统查询

+ 查询所有进程
`show processlist;`

+ 结束进程
`kill 9527`


## 用户和权限

+ 创建用户
`CREATE USER 'slkj'@'%' IDENTIFIED BY 'idx365';`

+ 删除用户
`DROP USER 'slkj'@'host';`

+ 更新ip绑定
`update user set host = 'localhost' where user = 'slkj' and host = '%';`

+ 查看所有用户
`select host, user from user;`

+ 查看权限
`show grants for 'slkj';`

+ 给用户 slkj 赋予所有库的所有权限  
`GRANT ALL PRIVILEGES ON *.* TO 'slkj'@'%' IDENTIFIED BY 'idx365' WITH GRANT OPTION;`

+ 重新载入赋权表  
`FLUSH PRIVILEGES;`
  
+ 收回权限(不包含赋权权限)
`REVOKE ALL PRIVILEGES ON slkj.* FROM slkj;  `

+ 收回赋权权限  
`REVOKE GRANT OPTION ON *.* FROM slkj;`


## 数据操作

+ 备份数据库
`mysqldump -ucloud -p index_cloud > data.sql;`

+ 备份存储过程
`mysqldump -uslkj -p -n -d -t -R spider_ncp > tong-pro.sql`

+ 查看存储过程列表
```
SELECT SPECIFIC_NAME
FROM information_schema.ROUTINES
WHERE BINARY ROUTINE_SCHEMA = 'demo' AND ROUTINE_TYPE = 'PROCEDURE';
```

+ 删除存储过程:
`DROP PROCEDURE demo.dali;`

+ 导出成csv文件

```
select *
from bm_whcd
into outfile '/tmp/test.csv'
fields terminated by ','
  optionally enclosed by '"'
  escaped by '"'
lines terminated by '\r\n';
```

## 其他

+ 显示警告(上次执行的语句)
`show warnings;`


+ 查看安全目录:
`SHOW VARIABLES LIKE 'secure_file_priv';`


