# 系统部分

## 数据库结构查询

+ 查询表格字段设计

```
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

    show processlist;

+ 结束进程

    kill 9527


## 存储过程操作

+ 备份存储过程

    mysqldump -uslkj -p -n -d -t -R spider_ncp > tong-pro.sql

+ 查看存储过程列表
```
SELECT SPECIFIC_NAME
FROM information_schema.ROUTINES
WHERE BINARY ROUTINE_SCHEMA = 'demo' AND ROUTINE_TYPE = 'PROCEDURE';
```

+ 删除存储过程:

    DROP PROCEDURE demo.dali;

## 其他

+ 显示警告(上次执行的语句)

    show warnings;

+ 查看安全目录:

    SHOW VARIABLES LIKE 'secure_file_priv';

## ONLY_FULL_GROUP_BY

在MySQL5.7+的版本中,默认开启了一项配置:`ONLY_FULL_GROUP_BY`.此项配置不允许在分组命令中显示其他列的数据.

查询是否开启
```
SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;
```

快速关闭
```
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

若要永久生效,则修改配置文件`/etc/my.cnf`,在`[mysqld]`下面添加:
```
[mysqld]  
sql_mode="STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
```

上面配置项的内容建议以当前数据库运行中的值为准
