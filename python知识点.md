# 数据库编程

#### 为什么需要数据库编程

数据持久化存储

#### DB-API

DB-API为不同的数据库提供了一致的接口，定义了必须的对象和数据库存储方式，python各个数据库模块需要兼容DB-API，包含几个对象：

1. 数据库连接对象：connection

2. 数据库交互对象：cursor（游标对象）

3. 数据库异常类：exceptions

数据库对象与数据库通信，有close、commit、rollback、cursor等方法。

游标对象可以执行数据库命令和得到查询结果，execute(sql)、fetchone()得到结果集的下一行、fetchmany(size)、fetchall(),在sql语句中，要确认一下数据类型，方式sql语句执行时类型不匹配

使用数据库的过程包括

1. 创建connection对象，获取cursor

2. 使用cursor执行SQL

3. 使用cursor获取数据、判断执行状态

4. 提交事务 或者 回滚事务

#### 对象-关系映射（ORM）

避免编写sql语句，将数据表转换为python类，它封装了数据库操作，只需面向对象。映射关系：

- 数据库的表（table） --> 类（class）
- 记录（record，行数据）--> 对象（object）
- 字段（field）--> 对象的属性（attribute）

ORM将数据库的增删改查封装成方法，表之间的关系设置为字段属性。