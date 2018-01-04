## ORM

ORM全称`Object Relational Mapping`,即"对象关系映射".

简单理解,就是为了实现`"某种开发语言(Java,Python,JavaScript...)"和"某种数据库(MySQL,Oracle...)" 之间的 数据映射(转换)`而开发出来的框架.

### 如果没有 ORM

+ 自己管理数据库连接的创建,使用和关闭.
+ 奇怪而繁琐的取值方式.
+ 到处出现的异常需要处理.
+ 手拼SQL语句: 复杂的传参方式 & 稍不注意便出现SQL漏洞注入.

### DbUtils

+ 解决了繁杂的数据转换操作.
+ 可以搭配数据库连接池.
+ 体积小巧,性能优秀.

但是:

+ 属性名无法自动转换.
+ 需要自己管理连接池.
+ 无法处理"一对多"等映射.

### ORM-Mybatis

+ 自动管理连接池.
+ 属性名自动转换.
+ 缓存.
+ 动态sql.
+ ...

### ORM-Hibernate

目前"指数云平台"和公司内其他SpringBoot框架都在使用JPA+Hibernate的组合.

>JPA

>`JPA`全称`Java Persistence API`.即"Java持久层API".是一个**规范**.

>它的目的是: 开发者只需要遵循此规范写查询代码,而无需关心真正的SQL是什么,甚至不用关心用的什么类型的数据库(MySQL,Oracle,Windows Server...).

Hibernate的用法省略.

### Node.js中的ORM: Sequelize

参考教程:
[廖雪峰JavaScript-使用Sequelize](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001471955049232be7492e76f514d45a2180e2c224eb7a6000)



