# mysql 数据库

## mysql系统配置

### 系统变量

在 MySQL 数据库，变量分为系统变量和用户自定义变量。系统变量以 @@ 开头，用户自定义变量以 @ 开头。

服务器维护着两种系统变量，即全局变量（GLOBAL VARIABLES）和会话变量（SESSION VARIABLES）。全局变量影响 MySQL 服务的整体运行方式，会话变量影响具体客户端连接的操作。

每一个客户端成功连接服务器后，都会产生与之对应的会话。会话期间，MySQL 服务实例会在服务器内存中生成与该会话对应的会话变量，这些会话变量的初始值是全局变量值的拷贝。

#### 查看系统变量

查看全局变量系信息：

`SHOW GLOBAL VARIABLES; `

查看与当前会话相关的所有会话变量以及全局变量。

`SHOW SESSION VARIABLES;`

#### 设置系统变量

1. 在 MySQL 配置文件（mysql.ini 或 mysql.cnf）中修改 MySQL 系统变量的值（需要重启 MySQL 服务才会生效）。
2. 在 MySQL 服务运行期间，使用 SET 命令重新设置系统变量的值。

### 存储引擎

InnoDB、MyISAM、Memory、Merge、Archive、CSV、BLACKHOLE 等。可以使用`SHOW ENGINES;`语句查看系统所支持的引擎类型。

#### 默认的搜索引擎

查看默认的存储引擎

`SHOW VARIABLES LIKE 'default_storage_engine%';`

修改数据库临时的默认存储引擎

`SET default_storage_engine=< 存储引擎名 >`

#### mysql 中的存储引擎

##### MyISAM

MyISAM 存储引擎不支持事务和外键，所以访问速度比较快。如果应用主要以读取和写入为主，只有少量的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择 MyISAM 存储引擎是非常适合的。

MyISAM 是在 Web 数据仓储和其他应用环境下最常使用的存储引擎之一。

##### InnoDB

  InnoDB 存储引擎在事务上具有优势，即支持具有提交、回滚和崩溃恢复能力的事务安装，所以比 MyISAM 存储引擎占用更多的磁盘空间。

如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询以外，还包括很多的更新、删除操作，那么 InnoDB 存储引擎是比较合适的选择。

InnoDB 存储引擎除了可以有效地降低由于删除和更新导致的锁定，还可以确保事务的完整提交（Commit）和回滚（Rollback），对于类似计费系统或者财务系统等对数据准确性要求比较高的系统，InnoDB 都是合适的选择。  

##### MEMORY

MEMORY 存储引擎将所有数据保存在 RAM 中，所以该存储引擎的数据访问速度快，但是安全上没有保障。

MEMORY 对表的大小有限制，太大的表无法缓存在内存中。由于使用 MEMORY 存储引擎没有安全保障，所以要确保数据库异常终止后表中的数据可以恢复。

如果应用中涉及数据比较少，且需要进行快速访问，则适合使用 MEMORY 存储引擎。

#### 修改存储引擎

`ALTER TABLE <表名> ENGINE=<存储引擎名>;`

### 大小写规则

SQL 语句的大小写规则与语句组成元素、引用内容和服务器所使用的操作系统有关。MySQL 在 Windows 系统下不区分大小写，但在 Linux 系统下默认区分大小写。因此，数据库名、表名和字段名，都不允许出现任何大写字母，避免节外生枝。

可以遵循 MySql 建表规约

```
【强制】表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑。
```

一般建议统一使用小写字母，并且 InnoDB 引擎在其内部都是以小写字母方式来存储数据库名和表名的。这样可以有效的防止 MySQL 产生大小写问题。

### 查看帮助

`HELP 查询内容`

## databases 操作

### 查看数据库

`SHOW DATABASES [LIKE '数据库名'];`

### 创建数据库

```sql
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];
```

MySQL 的字符集（CHARACTER）和校对规则（COLLATION）是两个不同的概念。字符集是用来定义 MySQL 存储字符串的方式，校对规则定义了比较字符串的方式。

### 查看 test_db_char 数据库的定义声明

`SHOW CREATE DATABASE 数据库名`

### 修改数据库

```sql
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
```

### 删除数据库

`DROP DATABASE [ IF EXISTS ] <数据库名>`

## 数据表操作

### 创建表

`CREATE TABLE <表名> ([表定义选项])[表选项][分区选项];`

### 修改表

```sql
ALTER TABLE <表名> [修改选项]
# 修改选项包括
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名>
| CHARACTER SET <字符集名>
| COLLATE <校对规则名> }
```

### 删除表

`DROP TABLE [IF EXISTS] 表名1 [ ,表名2, 表名3 ...]`

#### 存在外键关联时的删除

- 先删除与它关联的子表，再删除父表；但是这样会同时删除两个表中的数据。
- 将关联表的外键约束取消，再删除父表；适用于需要保留子表的数据，只删除父表的情况。

### 查看表结构

```
DESC <表名>;
# 以表格的形式展示表结构
SHOW CREATE TABLE <表名>;
# 以SQL语句的形式展示表结构
还可以通过\g或者\G参数来控制展示格式。

```

### 表中添加字段

#### 在末尾添加字段

`ALTER TABLE <表名> ADD <新字段名><数据类型>[约束条件];`

#### 表头添加字段

`ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] FIRST;`

#### 指定的字段之后添加字段

`ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] AFTER <已经存在的字段名>;`

## mysql 功能

### 字段约束

#### 主键约束

主键是表的一个特殊字段，该字段能唯一标识该表中的每条信息。使用最频繁。

使用主键应注意以下几点：

- 每个表只能定义一个主键。
- 主键值必须唯一标识表中的每一行，且不能为 NULL，即表中不可能存在有相同主键值的两行数据。这是唯一性原则。
- 一个字段名只能在联合主键字段表中出现一次。
- 联合主键不能包含不必要的多余字段。当把联合主键的某一字段删除后，如果剩下的字段构成的主键仍然满足唯一性原则，那么这个联合主键是不正确的。这是最小化原则。

添加主键约束方法

```
<字段名> <数据类型> PRIMARY KEY [默认值] 
# 创建表时，在定义字段的同时指定主键，设置单字段主键

[CONSTRAINT <约束名>] PRIMARY KEY [字段名]
# 在定义完所有字段之后指定主键

PRIMARY KEY [字段1，字段2，…,字段n]
# 创建表时设置联合主键

ALTER TABLE <数据表名> ADD PRIMARY KEY(<字段名>);
# 修改数据表时添加主键约束

ALTER TABLE <数据表名> DROP PRIMARY KEY;
# 删除主键约束
```

##### AUTO_INCREMENT 实现主键自增长

当主键定义为自增长后，这个主键的值就不再需要用户输入数据了，而由数据库系统根据定义自动赋值。每增加一条记录，主键会自动以相同的步长进行增长。

```
字段名 数据类型 AUTO_INCREMENT
# 给字段添加 AUTO_INCREMENT 属性来实现主键自增长

AUTO_INCREMENT=100;
# 指定自增加的初始值

show global variables like 'auto_incre%';
set global auto_increment_increment=20;
# 设置自增长步长
```



#### 外键约束

外键约束经常和主键约束一起使用，用来确保数据的一致性。

定义规则：

- 主表必须已经存在于数据库中，或者是当前正在创建的表。如果是后一种情况，则主表与从表是同一个表，这样的表称为自参照表，这种结构称为自参照完整性。
- 必须为主表定义主键。
- 主键不能包含空值，但允许在外键中出现空值。也就是说，只要外键的每个非空值出现在指定的主键中，这个外键的内容就是正确的。
- 在主表的表名后面指定列名或列名的组合。这个列或列的组合必须是主表的主键或候选键。
- 外键中列的数目必须和主表的主键中列的数目相同。
- 外键中列的数据类型必须和主表主键中对应列的数据类型相同。

外键约束操作：

```
[CONSTRAINT <外键名>] FOREIGN KEY 字段名 [，字段名2，…]
REFERENCES <主表名> 主键列1 [，主键列2，…]
# 创建表时设置外键约束

ALTER TABLE <数据表名> ADD CONSTRAINT <外键名>
FOREIGN KEY(<列名>) REFERENCES <主表名> (<列名>);
# 修改表时添加外键约束

ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;
# 删除外键约束
```



#### 唯一约束

唯一约束与主键约束有一个相似的地方，就是它们都能够确保列的唯一性。唯一约束在一个表中可有多个,与主键约束不同的是，唯一约束在一个表中可以有多个，并且设置唯一约束的列是允许有空值的，虽然只能有一个空值。

唯一约束操作

```sql
<字段名> <数据类型> UNIQUE
# 创建表时添加约束

ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
# 修改表时添加

ALTER TABLE <表名> DROP INDEX <唯一约束名>;
# 删除唯一约束
```

#### 检查约束

用来检查数据表中，字段值是否有效的一个手段。默认值约束和非空约束也可看作是特殊的检查约束。若将 CHECK 约束子句置于所有列的定义以及主键约束和外键定义之后，则这种约束也称为基于表的 CHECK 约束。该约束可以同时对表中多个列设置限定条件。

检查约束操作：

```
mysql> CREATE TABLE tb_emp7
    -> (
    -> id INT(11) PRIMARY KEY,
    -> name VARCHAR(25),
    -> deptId INT(11),
    -> salary FLOAT,
    -> CHECK(salary>0 AND salary<100),
    -> FOREIGN KEY(deptId) REFERENCES tb_dept1(id)
    -> );
# 创建表时设置检查约束

ALTER TABLE tb_emp7 ADD CONSTRAINT <检查约束名> CHECK(<检查约束>)
# 修改表示创建约束

ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;
# 删除检查约束
```

#### 非空约束

用来约束表中的字段不能为空。

非空约束操作：

```
<字段名> <数据类型> NOT NULL;
# 创建表时设置非空约束

ALTER TABLE <数据表名>
CHANGE COLUMN <字段名>
<字段名> <数据类型> NOT NULL;
# 修改表时添加非空约束

ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
# 删除非空约束
```

#### 默认值约束

用来约束当数据表中某个字段不输入值时，自动为其添加一个已经设置好的值。默认值约束通常用在已经设置了非空约束的列，这样能够防止数据表在录入数据时出现错误。

默认值约束操作：

```
<字段名> <数据类型> DEFAULT <默认值>;
# 创建表时设置默认值约束

ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
# 修改表时添加默认值约束

ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
# 删除默认值约束
```

### mysql 运算符

##### 算术运算符

加、减、乘、除和取余运算

##### 逻辑运算符

逻辑非运算符：`NOT`和`!`

- 当操作数为 0（假）时，返回值为 1；
- 当操作数为非零值时，返回值为 0；
- 当操作数为 NULL 时，返回值为 NULL。

逻辑与：AND 或者 &&

- 当所有操作数都为非零值并且不为 NULL 时，返回值为 1；
- 当一个或多个操作数为 0 时，返回值为 0；
- 操作数中有任何一个为 NULL 时，返回值为 NULL。

逻辑或：OR 和 ||

- 当两个操作数都为非 NULL 值时，如果有任意一个操作数为非零值，则返回值为 1，否则结果为 0；
- 当有一个操作数为 NULL 时，如果另一个操作数为非零值，则返回值为 1，否则结果为NULL；
- 假如两个操作数均为 NULL 时，则返回值为 NULL。

逻辑异或：XOR

- 当任意一个操作数为 NULL 时，返回值为 NULL；
- 对于非 NULL 的操作数，如果两个操作数都是非 0 值或者都是 0 值，则返回值为 0；
- 如果一个为0值，另一个为非 0 值，返回值为 1。

##### 比较运算符

= 运算符用来比较两边的操作数是否相等，相等的话返回 1，不相等的话返回 0。

- 若有一个或两个操作数为 NULL，则比较运算的结果为 NULL。
- 若两个操作数都是字符串，则按照字符串进行比较。
- 若两个操作数均为整数，则按照整数进行比较。
- 若一个操作数为字符串，另一个操作数为数字，则 MySQL 可以自动将字符串转换为数字。

<=> 操作符和 = 操作符类似，不过 <=> 可以用来判断 NULL 值。

<> 和 != 用于判断数字、字符串、表达式是否不相等。

<= 是小于等于运算符，用来判断左边的操作数是否小于或者等于右边的操作数。

\>  \>=  < 相同

IS NULL 或 ISNULL 运算符用来检测一个值是否为 NULL。

IS NOT NULL 运算符用来检测一个值是否为非 NULL。

BETWEEN AND 运算符用来判断表达式的值是否位于两个数之间，或者说是否位于某个范围内，用法

```
expr BETWEEN min AND max
# expr 表示要判断的表达式，min 表示最小值，max 表示最大值。如果 expr 大于等于 min 并且小于等于 max，那么返回值为 1，否则返回值为 0。
```

##### 位运算符 

位或运算符： |，有一个为 1 时，结果就为 1，两个都为 0 时结果才为 0。

位与运算符： &，两个二进制位都为 1 时，结果就为 1，否则为 0。

位异或运算符：^，两个二进制位不同时，结果为 1，相同时，结果为 0。

位左移运算符：<<，按指定值的补码形式进行左移，左移指定位数之后，左边高位的数值被移出并丢弃，右边低位空出的位置用 0 补齐。

位右移运算符：>>，进行右移，右移指定位数之后，右边低位的数值被移出并丢弃，左边高位空出的位置用 0 补齐。

位取反运算符：~，将参与运算的数据按对应的补码进行反转，也就是做 NOT 操作，即 1 取反后变 0，0 取反后变为 1。

##### IN 和 NOT IN

 IN 运算符用来判断表达式的值是否位于给出的列表中；如果是，返回值为 1，否则返回值为 0。NOT IN 的作用和 IN 恰好相反。

语法格式：

```
expr IN ( value1, value2, value3 ... valueN )
expr NOT IN ( value1, value2, value3 ... valueN )
```

NULL 值处理

当 IN 运算符的两侧有一个为空值 NULL 时，如果找不到匹配项，则返回值为 NULL；如果找到了匹配项，则返回值为 1。NOT IN 恰好相反，当 NOT IN 运算符的两侧有一个为空值 NULL 时，如果找不到匹配项，则返回值为 NULL；如果找到了匹配项，则返回值为 0。

### mysql 函数

MySQL 函数包括数学函数、字符串函数、日期和时间函数、条件判断函数、系统信息函数和加密函数等。

SELECT、INSERT、UPDATE 和 DELETE 语句及其子句（例如 WHERE、ORDER BY、HAVING 等）中都可以使用 MySQL 函数。

常用函数：http://c.biancheng.net/mysql/function/

## sql 查询

```sql
SELECT 
DISTINCT <select_list>
FROM <left_table>
<join_type> JOIN <right_table>
ON <join_condition>
WHERE <where_condition>
GROUP BY <group_by_list>
HAVING <having_condition>
ORDER BY <order_by_condition>
LIMIT <limit_number>
```

### 查询字段

`SELECT < 列名 > FROM < 表名 >;`

### 过滤查询字段

`SELECT DISTINCT <字段名> FROM <表名>;`

"字段名”为需要消除重复记录的字段名称，多个字段时用逗号隔开。

使用 DISTINCT 关键字时需要注意以下几点：

- DISTINCT 关键字只能在 SELECT 语句中使用。

- 在对一个或多个字段去重时，DISTINCT 关键字必须在所有字段的最前面。

- 如果 DISTINCT 关键字后有多个字段，则会对多个字段进行组合去重，也就是说，只有多个字段组合起来完全是一样的情况下才会被去重。

### 指定别名

```
SELECT stu.name,stu.height FROM tb_students_info AS stu;
# 为表指定别名

SELECT name AS student_name, age AS student_age FROM tb_students_info;
# 为字段指定别名
```

表的别名不能与该数据库的其它表同名。字段的别名不能与该表的其它字段同名。

在条件表达式中不能使用字段的别名，否则会出现“ERROR 1054 (42S22): Unknown column”这样的错误提示信息。

### 限制查询条数

三种用法：

指定初始位置：`LIMIT 初始位置，记录数`

不指定初始位置：LIMIT 记录数，带一个参数的 LIMIT 指定从查询结果的首行开始。

和 OFFSET 组合使用：LIMIT 记录数 OFFSET 初始位置

### 排序

ORDER BY 关键字主要用来将查询结果中的数据按照一定的顺序进行排序。

`ORDER BY <字段名> [ASC|DESC]`

- 字段名：表示需要排序的字段名称，多个字段时用逗号隔开。
- ASC|DESC：`ASC`表示字段按升序排序；`DESC`表示字段按降序排序。其中`ASC`为默认值。

使用 ORDER BY 关键字应该注意以下几个方面：

- ORDER BY 关键字后可以跟子查询 。
- 当排序的字段中存在空值时，ORDER BY 会将该空值作为最小值来对待。
- ORDER BY 指定多个字段进行排序时，MySQL 会按照字段的顺序从左到右依次进行排序。

### 条件查询数据

`WHERE 查询条件`

查询条件可以是：

- 带比较运算符和逻辑运算符的查询条件
- 带 BETWEEN AND 关键字的查询条件
- 带 IS NULL 关键字的查询条件
- 带 IN 关键字的查询条件
- 带 LIKE 关键字的查询条件

单一条件查询：

```
mysql> SELECT name,height FROM tb_students_info
    -> WHERE height=170;
```

多条件查询： WHERE 关键词后可以有多个查询条件，这样能够使查询结果更加精确。多个查询条件时用逻辑运算符 AND（&&）、OR（||）或 XOR 隔开。

```
mysql> SELECT name,age,height FROM tb_students_info 
    -> WHERE age>21 AND height>=175;
```

### 模糊查询

LIKE 关键字主要用于搜索匹配字段中的指定内容，

`[NOT] LIKE  '字符串'`

- NOT ：可选参数，字段中的内容与指定的字符串不匹配时满足条件。
- 字符串：指定用来匹配的字符串。“字符串”可以是一个很完整的字符串，也可以包含通配符。

LIKE 关键字支持百分号“%”和下划线“_”通配符。

“%”是 MySQL 中最常用的通配符，它能代表任何长度的字符串，字符串的长度可以为 0。

“_”只能代表单个字符，字符的长度不能为 0。

默认情况下，LIKE 关键字匹配字符的时候是不区分大小写的。如果需要区分大小写，可以加入 BINARY 关键字。

```
SELECT name FROM tb_students_info WHERE name LIKE 't%';
# 查找以 t 或 T 开头的

SELECT name FROM tb_students_info WHERE name LIKE BINARY 't%';
# 区分大小写，以 t 开头
```

### 范围查询

BETWEEN AND 关键字用来判断字段的数值是否在指定范围内。

`[NOT] BETWEEN 取值1 AND 取值2`

- NOT：可选参数，表示指定范围之外的值。如果字段值不满足指定范围内的值，则这些记录被返回。
- 取值1：表示范围的起始值。
- 取值2：表示范围的终止值。

### 判断是否是空值

IS NULL 关键字，用来判断字段的值是否为空值（NULL）。空值不同于 0，也不同于空字符串。

IS NULL 是一个整体，不能将 IS 换成“=”。

IS NOT NULL 

表示查询字段值不为空的记录。

### 分组查询

查看 `@@global.sql_mode`中是否包括`ONLY_FULL_GROUP_BY`，此选项意思为，对于group by聚合操作,如果在select中的列没有在group by中出现,那么这个SQL是不合法的,因为列不在group by从句中 。

`GROUP BY  <字段名>`

“字段名”表示需要分组的字段名称，多个字段时用逗号隔开,可以根据一个或多个字段对查询结果进行分组。

GROUP BY 关键字可以和 GROUP_CONCAT() 函数一起使用。GROUP_CONCAT() 函数会把每个分组的字段值都显示出来。

```sql
 select GROUP_CONCAT(id),`salary` from employer group by salary;
 +------------------+--------+
| GROUP_CONCAT(id) | salary |
+------------------+--------+
| 1,2              |     10 |
| 3                |     11 |
| 4                |     12 |
| 5                |     13 |
| 6                |     14 |
| 7                |     15 |
+------------------+--------+
```

在数据统计时，GROUP BY 关键字经常和聚合函数一起使用。包括 COUNT()，SUM()，AVG()，MAX() 和 MIN()。

WITH POLLUP 关键字用来在所有记录的最后加上一条记录，这条记录是上面所有记录的总和，即统计记录数量。

```
select count(salary) as num,`salary`  from employer group by salary;
# 按 salary 分组，每组几人

select count(salary) as num,`salary`  from employer group by salary WITH ROLLUP;
# 添加最后一条总和记录。
+-----+--------+
| num | salary |
+-----+--------+
|   2 |     10 |
|   1 |     11 |
|   1 |     12 |
|   1 |     13 |
|   1 |     14 |
|   1 |     15 |
|   7 |   NULL |
+-----+--------+
```

### HAVING 过滤分组

HAVING 关键字对分组后的数据进行过滤。HAVING 关键字和 WHERE 关键字都可以用来过滤数据，且 HAVING 支持 WHERE 关键字中所有的操作符和语法。存在的差异为：

- 一般情况下，WHERE 用于过滤数据行，而 HAVING 用于过滤分组。
- WHERE 查询条件中不可以使用聚合函数，而 HAVING 查询条件中可以使用聚合函数。
- WHERE 在数据分组前进行过滤，而 HAVING 在数据分组后进行过滤 。
- WHERE 针对数据库文件进行过滤，而 HAVING 针对查询结果进行过滤。也就是说，WHERE 根据数据表中的字段直接进行过滤，而 HAVING 是根据前面已经查询出的字段进行过滤。
- WHERE 查询条件中不可以使用字段别名，而 HAVING 查询条件中可以使用字段别名。

### 多表查询

多表查询就是同时查询两个或两个以上的表，主要有交叉连接、内连接和外连接。

#### 交叉连接

返回连接表的笛卡尔积。

笛卡尔积：指两个集合 X 和 Y 的乘积。

```
A = {1,2}
B = {3,4,5}
A×B={(1,3), (1,4), (1,5), (2,3), (2,4), (2,5) };
B×A={(3,1), (3,2), (4,1), (4,2), (5,1), (5,2) };
```

交叉连接形式：

```
SELECT <字段名> FROM <表1> CROSS JOIN <表2> [WHERE子句]
或者
SELECT <字段名> FROM <表1>, <表2> [WHERE子句] 
```

- 字段名：需要查询的字段名称。
- <表1><表2>：需要交叉连接的表名。
- WHERE 子句：用来设置交叉连接的查询条件。

#### 内连接

通过设置连接条件的方式，来移除查询结果中某些数据行的交叉连接。

内连接使用 **INNER JOIN** 关键字连接两张表，并使用 ON 子句来设置连接条件。如果没有连接条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。

格式：

`SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]`

- 字段名：需要查询的字段名称。
- <表1><表2>：需要内连接的表名。
- INNER JOIN ：内连接中可以省略 INNER 关键字，只用关键字 JOIN。
- ON 子句：用来设置内连接的连接条件。

当对多个表进行查询时，要在 SELECT 语句后面指定字段是来源于哪一张表。因此，在多表查询时，SELECT 语句后面的写法是`表名.列名`。另外，如果表名非常长的话，也可以给表设置别名，这样就可以直接在 SELECT 语句后面写上`表的别名.列名`。

```
mysql> select s.name,e.salary from
    -> students s inner join employer e
    -> on s.sid = e.id;
+------+--------+
| name | salary |
+------+--------+
| xrb  |     10 |
| zzz  |     10 |
+------+--------+
```

#### 外连接

内连接的查询结果都是符合连接条件的记录，而外连接会先将连接的表分为基表和参考表，再以基表为依据返回满足和不满足条件的记录。外连接可以分为左外连接和右外连接。

##### 左连接

左外连接又称为左连接，使用 **LEFT OUTER JOIN** 关键字连接两个表，并使用 ON 子句来设置连接条件。

`SELECT <字段名> FROM <表1> LEFT OUTER JOIN <表2> <ON子句>`

- 字段名：需要查询的字段名称。
- <表1><表2>：需要左连接的表名。
- LEFT OUTER JOIN：左连接中可以省略 OUTER 关键字，只使用关键字 LEFT JOIN。
- ON 子句：用来设置左连接的连接条件，不能省略。

“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）。

```
 select s.name,e.salary from employer e left join students s on s.sid = e.id;
 +------+--------+ 
| name | salary | 
+------+--------+ 
| xrb  |     10 | 
| zzz  |     10 | 
| NULL |     11 | 
| NULL |     12 | 
| NULL |     13 | 
| NULL |     14 | 
| NULL |     15 | 
+------+--------+ 
```

##### 右连接

右外连接又称为右连接，右连接是左连接的反向连接。使用 **RIGHT OUTER JOIN** 关键字连接两个表，并使用 ON 子句来设置连接条件。

`SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>`

- 字段名：需要查询的字段名称。
- <表1><表2>：需要右连接的表名。
- RIGHT OUTER JOIN：右连接中可以省略 OUTER 关键字，只使用关键字 RIGHT JOIN。
- ON 子句：用来设置右连接的连接条件，不能省略。

与左连接相反，右连接以“表2”为基表，“表1”为参考表。右连接查询时，可以查询出“表2”中的所有记录和“表1”中匹配连接条件的记录。如果“表2”的某行在“表1”中没有匹配行，那么在返回结果中，“表1”的字段值均为空值（NULL）。

```
select s.name,e.salary from students s right join employer e on s.sid = e.id;
+------+--------+
| name | salary |
+------+--------+
| xrb  |     10 |
| zzz  |     10 |
| NULL |     11 |
| NULL |     12 |
| NULL |     13 |
| NULL |     14 |
| NULL |     15 |
+------+--------+
```

### 子查询

通过子查询可以实现多表查询。子查询指将一个查询语句嵌套在另一个查询语句中。子查询可以在 SELECT、UPDATE 和 DELETE 语句中使用，而且可以进行多层嵌套。在实际开发时，子查询经常出现在 WHERE 子句中。

`WHERE <表达式> <操作符> (子查询)`

操作符可以是比较运算符和 IN、NOT IN、EXISTS、NOT EXISTS 等关键字。

1）IN | NOT IN

当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回值正好相反。

2）EXISTS | NOT EXISTS

用于判断子查询的结果集是否为空，若子查询的结果集不为空，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。

```
ysql> select name,sex,sid from students
    -> where sid in (select id from employer);
```

外层的 SELECT 查询称为父查询，圆括号中嵌入的查询称为子查询（子查询必须放在圆括号内）。MySQL 在处理上例的 SELECT 语句时，执行流程为：先执行子查询，再执行父查询。

子查询的功能也可以通过表连接完成，但是子查询会使 SQL 语句更容易阅读和编写。

一般来说，表连接（内连接和外连接等）都可以用子查询替换，但反过来却不一定，有的子查询不能用表连接来替换。子查询比较灵活、方便、形式多样，适合作为查询的筛选条件，而表连接更适合于查看连接表的数据。  

子查询的注意事项

1) 子查询语句可以嵌套在 SQL 语句中任何表达式出现的位置

可以被嵌套在 SELECT 语句的列、表和查询条件中，即 SELECT 子句，FROM 子句、WHERE 子句、GROUP BY 子句和 HAVING 子句。

嵌套在 SELECT 子句中：

`SELECT (子查询) FROM 表名;`

子查询结果为单行单列，但不必指定列别名。

嵌套在 FROM 子句中：

`SELECT * FROM (子查询) AS 表的别名;`

必须为表指定别名。一般返回多行多列数据记录，可以当作一张临时表。

2) 只出现在子查询中而没有出现在父查询中的表不能包含在输出列中

多层嵌套子查询的最终数据集只包含父查询（即最外层的查询）的 SELECT 子句中出现的字段，而子查询的输出结果通常会作为其外层子查询数据源或用于数据判断匹配。

#### 子查询改写为表连接

与表连接相比，子查询比较灵活，方便，形式多样，适合作为查询的筛选条件，而表连接更适合查看多表的数据。在检查那些倾向于编写成子查询的查询语句时，可以考虑将子查询替换为表连接.

1. 改写用来查询匹配值的子查询

```sql
SELECT * FROM table1
WHERE column1 IN (SELECT column2a FROM table2 WHERE column2b = value);

column1 代表 table1 中的字段，column2a 和 column2b 代表 table2 表中的字段。这类查询都可以被转换为下面这种形式的连接查询：

SELECT table1. * FROM table1 INNER JOIN table2
ON table1. column1 = table. column2a WHERE table2. column2b = value;
```

2. 改写用来查询非匹配（缺失）值的子查询

另一种常见的子查询语句类型是：把存在于某个表里，但在另一个表里并不存在的那些值查找出来。“哪些值不存在”有关的问题通常都可以用 LEFT JOIN 来解决。

```
SELECT * FROM table1
WHERE column1 NOT IN ( SELECT column2 FROM table2) ;

改写为

SELECT table1.* FROM table1 LEFT JOIN table2
ON table1.column1 = table2.column2 WHERE table2.column2 IS NULL;
```

### 正则表达式查询

正则表达式主要用来查询和替换符合某个模式（规则）的文本内容。

`属性名 REGEXP '匹配方式'`

“属性名”表示需要查询的字段名称；“匹配方式”表示以哪种方式来匹配查询。“匹配方式”中有很多的模式匹配字符，它们分别表示不同的意思。

### sql 查询顺序

sql 查询时的执行顺序为

```
FROM
<表名> # 笛卡尔积
ON
<筛选条件> # 对笛卡尔积的虚表进行筛选
JOIN <join, left join, right join...> 
<join表> # 指定join，用于添加数据到on之后的虚表中，例如left join会将左表的剩余数据添加到虚表中
WHERE
<where条件> # 对上述虚表进行筛选
GROUP BY
<分组条件> # 分组
<SUM()等聚合函数> # 用于having子句进行判断，在书写上这类聚合函数是写在having判断里面的
HAVING
<分组筛选> # 对分组后的结果进行聚合筛选
SELECT
<返回数据列表> # 返回的单列必须在group by子句中，聚合函数除外
DISTINCT
# 数据除重
ORDER BY
<排序条件> # 排序
LIMIT
<行数限制>
```

**引擎在执行上述每一步时，都会在内存中形成一张虚拟表，然后对虚拟表进行后续操作**，并释放没用的虚拟表的内存。每一步具体为：

1.  from：**select \* from table_1, table_2; 与 select \* from table_1 join table_2; 的结果一致，都是表示求笛卡尔积；**用于直接计算两个表笛卡尔积，得到虚拟表VT1，这是所有select语句最先执行的操作，其他操作时在这个表上进行的，也就是from操作所完成的内容
2. on: 从VT1表中筛选符合条件的数据，形成VT2表；
3. join: 将该 join 类型的数据补充到VT2表中，**例如 left join 会将左表的剩余数据添加到虚表VT2中，形成VT3表；若表的数量大于2，则会重复1-3步**；
4. where: 执行筛选，（不能使用聚合函数）得到VT4表；
5. group by: 对VT4表进行分组，得到VT5表；其后处理的语句，如select，having，所用到的列必须包含在group by条件中，没有出现的需要用聚合函数；
6. having: 筛选分组后的数据，得到VT6表；
7. select: 返回列得到VT7表；
8. distinct: 用于去重得到VT8表；
9. order by: 用于排序得到VT9表；
10. limit: 返回需要的行数，得到VT10；

## 添加数据

### 1) INSERT…VALUES语句

```
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];
```

- `<表名>`：指定被操作的表名。
- `<列名>`：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT<表名>VALUES(…) 即可。
- `VALUES` 或 `VALUE` 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。

### 2) INSERT…SET语句

```
INSERT INTO <表名>
SET <列名1> = <值1>,
        <列名2> = <值2>,
        …
```

- 使用 INSERT…VALUES 语句可以向表中插入一行数据，也可以插入多行数据；
- 使用 INSERT…SET 语句可以指定插入行中每列的值，也可以指定部分列的值；
- INSERT…SELECT 语句向表中插入其他表的数据。
- 采用 INSERT…SET 语句可以向表中插入部分列的值，这种方式更为灵活；
- INSERT…VALUES 语句可以一次插入多条数据。

## 修改数据

#### 修改表字段数据

```
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ]
[ORDER BY 子句] [LIMIT 子句]
```

- `<表名>`：用于指定要更新的表名称。
- `SET` 子句：用于指定表中要修改的列名及其列值。其中，每个指定的列值可以是表达式，也可以是该列对应的默认值。如果指定的是默认值，可用关键字 DEFAULT 表示列值。
- `WHERE` 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
- `ORDER BY` 子句：可选项。用于限定表中的行被修改的次序。
- `LIMIT` 子句：可选项。用于限定被修改的行数。

#### 多表关联更新

对于多表的 UPDATE 操作需要慎重，建议在更新前，先使用 SELECT 语句查询验证更新的数据与自己期望的是否一致。

1. 使用UPDATE

```
mysql> UPDATE product p, product_price pp SET pp.price = p.price * 0.8 WHERE p.productid= pp.productId;
```

2. 通过 INNER JOIN

```
mysql> UPDATE product p INNER JOIN product_price pp ON p.productid= pp.productid SET pp.price = p.price * 0.8;
```

3. 通过 LEFT JOIN

```
mysql> UPDATE product p LEFT JOIN product_price pp ON p.productid= pp.productid SET p.isdelete = 1 WHERE pp.productid IS NULL;
```

4. 通过子查询

```
mysql> UPDATE product_price pp SET price=(SELECT price*0.8 FROM product WHERE productid = pp.productid);
```



## 删除数据

```
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
```

- `<表名>`：指定要删除数据的表名。
- `ORDER BY` 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。
- `WHERE` 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。
- `LIMIT` 子句：可选项。用于告知服务器在控制命令被返回到客户端前被删除行的最大值。

## 清空表

DELETE 和 TRUNCATE 关键字来删除表中的数据。

```
TRUNCATE [TABLE] 表名 
清空一个表
```

从逻辑上说，TRUNCATE 语句与 DELETE 语句作用相同，但是在某些情况下，两者在使用上有所区别。

- DELETE 是 DML 类型的语句；TRUNCATE 是 DDL 类型的语句。它们都用来清空表中的数据。
- DELETE 是逐行一条一条删除记录的；TRUNCATE 则是直接删除原来的表，再重新创建一个一模一样的新表，而不是逐行删除表中的数据，执行数据比 DELETE 快。因此需要删除表中全部的数据行时，尽量使用 TRUNCATE 语句， 可以缩短执行时间。
- DELETE 删除数据后，配合事件回滚可以找回数据；TRUNCATE 不支持事务的回滚，数据删除后无法找回。
- DELETE 删除数据后，系统不会重新设置自增字段的计数器；TRUNCATE 清空表记录后，系统会重新设置自增字段的计数器。
- DELETE 的使用范围更广，因为它可以通过 WHERE 子句指定条件来删除部分数据；而 TRUNCATE 不支持 WHERE 子句，只能删除整体。
- DELETE 会返回删除数据的行数，但是 TRUNCATE 只会返回 0，没有任何意义。

## 索引

索引是一种特殊的数据库结构，由数据表中的一列或多列组合而成，可以用来快速查询数据表中有某一特定值的记录。

索引的优点如下：

- 通过创建唯一索引可以保证数据库表中每一行数据的唯一性。
- 可以给所有的 MySQL 列类型设置索引。
- 可以大大加快数据的查询速度，这是使用索引最主要的原因。
- 在实现数据的参考完整性方面可以加速表与表之间的连接。
- 在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间

缺点有：

- 创建和维护索引组要耗费时间，并且随着数据量的增加所耗费的时间也会增加。
- 索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间。如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。
- 当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，这样就降低了数据的维护速度。

  MySQL目前主要有的索引类型为：普通索引、唯一索引、主键索引、组合索引、全文索引。

### 创建索引

1) 使用 CREATE INDEX 语句

```
CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC])
```

- `<索引名>`：指定索引名。一个表可以创建多个索引，但每个索引在该表中的名称是唯一的。
- `<表名>`：指定要创建索引的表名。
- `<列名>`：指定要创建索引的列名。通常可以考虑将查询语句中在 JOIN 子句和 WHERE 子句里经常出现的列作为索引列。
- `<长度>`：可选项。指定使用列前的 length 个字符来创建索引。使用列的一部分创建索引有利于减小索引文件的大小，节省索引列所占的空间。在某些情况下，只能对列的前缀进行索引。索引列的长度有一个最大上限 255 个字节（MyISAM 和 InnoDB 表的最大上限为 1000 个字节），如果索引列的长度超过了这个上限，就只能用列的前缀进行索引。另外，BLOB 或 TEXT 类型的列也必须使用前缀索引。
- `ASC|DESC`：可选项。`ASC`指定索引按照升序来排列，`DESC`指定索引按照降序来排列，默认为`ASC`。

2) 使用 CREATE TABLE 语句

```
CONSTRAINT PRIMARY KEY [索引类型] (<列名>,…)
# 在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的主键。

KEY | INDEX [<索引名>] [<索引类型>] (<列名>,…)
在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的索引。

UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的唯一性索引。

FOREIGN KEY <索引名> <列名>
在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的外键。

```

3) 使用 ALTER TABLE 语句

```
ADD INDEX [<索引名>] [<索引类型>] (<列名>,…)
```

### 查看索引

```
SHOW INDEX FROM <表名> [ FROM <数据库名>]
```

- <表名>：指定需要查看索引的数据表名。
- <数据库名>：指定需要查看索引的数据表所在的数据库，可省略。比如，SHOW INDEX FROM student FROM test; 语句表示查看 test 数据库中 student 数据表的索引。

### 删除索引

1) 使用 DROP INDEX 语句

```
DROP INDEX <索引名> ON <表名>
```

2) 使用 ALTER TABLE 语句

```
将 ALTER TABLE 语句的语法中部分指定为以下子句中的某一项。
DROP PRIMARY KEY：表示删除表中的主键。一个表只有一个主键，主键也是一个索引。
DROP INDEX index_name：表示删除名称为 index_name 的索引。
DROP FOREIGN KEY fk_symbol：表示删除外键。
```

### 索引无效的情况

可以使用 EXPLAIN 来查看查询了多少行。

1. 在查询语句中使用 LIKE 关键字进行查询时，如果匹配字符串的第一个字符为“%”，索引不会被使用。
2. 表的多个字段上创建一个索引，只有查询条件中使用了这些字段中的第一个字段，索引才会被使用。
3. 查询语句只有 OR 关键字时，如果 OR 前后的两个条件的列都是索引，那么查询中将使用索引。如果 OR 前后有一个条件的列不是索引，那么查询中将不使用索引。