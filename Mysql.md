什么是数据库：专门搞数据管理的一种软件，可以通过网络，提供远程的数据管理服务，也称数据库服务器

| 基于键值对 | 适用于缓存             | redis、memcached |
| ---------- | ---------------------- | ---------------- |
| 基于文档型 | 适用于存储网页         | mongodb          |
| 基于列族   | 适用于大数据，数据分析 | hbase            |
| 基于图形   | 关系非常复杂的情况     | neo4j            |

# SQL分类

1. DDl：数据定义语言

   定义表和字段（create、drop、alter等）

2. DML：数据操作语言

   做数据操作的：添加修改删除（insert、delete、update等）

   ​	DQL：数据查询语言，属于DML中的一种 （select）

3. DCL数据控制语言、主要负责权限管理（grant——赋予权限、revoke——撤销权限）

   ​														和事务（commit——提交事务、rollback——回滚事务等）

# 1、数据库操作

> []  中为可选项

### 1.1查询/显示所有数据库

```
show database;
```

### 1.2创建数据库

```
create database [if not exist] 'db_name' character set utf8mb4; 
```

### 1.3使用数据库

```
use ‘db_name’  //先进入数据库，在使用表和数据
```

### 1.4删除数据库（慎重）

```
drop database [if exist]'db_name'  //必须有数据库才可以删，否则报错
```



# 2、常用的数据类型

### 2.1数值类型

分为整形和浮点型

bit（n）	二进制数据 最多n位

int 四字节

bigint 八字节

float（m，d）

double（m，d）

> m:总长度，整数位长度+小数位长度
>
> d：小数位长度
>
> 底层整数和小数长度有限，数字相除操作，导致小数位太长，会造成精度丢失

decimal（m，d）

numerical（m，d）

> 双精度浮点类型，精度要求高，商品价格，存款金额等

### 2.2字符串类型

varchar（size）	存储最多n位字符串（超出长度报错）

text 	一般存储大文本，比如纹章内容

> 最多存储65535，超出可使用mediumtext

### 2.3日期类型

datetime	八字节	不带时区，范围大

timestamp	四字节   带时区，范围小



# 3、常用表操作

### 显示数据库中所有表

```
show tables
```

### 3.1查看表结构

```
desc '表命'
```

| 字段名 | 字段类型 | null约束 | 索引 |
| ------ | -------- | -------- | ---- |
|        |          |          |      |

### 3.2创建表

```
create table table_name(
	字段名  字段数据类型，  //字段之间用逗号隔开，最后一个没有逗号

);
```

> - cmd默认选中文本就会复制
> - 建库、建表可以先删除、在创建
> - -- 注释（内容忽略）
> - ==数据库名、表名、字段名不能是数据库关键字==，如果需要定义成关键字可以使用**``**(数字1左边的符号)
> - comment ’ ‘    添加注释，用单引号（内容会保存的注释）
> - 字符串需要使用单引号包裹

###  3.3删除表

```
drop table if exist table_name;
```

### 4.数据操作   增删改查（CRUD）

#### 插入数据

全列插入：insert into 表名  values（字段1内容，字段2内容）

> 全列插入，表示所有字段都要插入值，且插入数据与定义时相同）

指定列插入：insert into 表名（待插入字段1，待插入字段2） value （要插入的值1，要插入的值2);   

> 只插入需要字段，其余为空。

多行插入：

1. 执行多条insert语句

2. insert into 表名 (待插入字段1，待插入字段2）value

   ​				(要插入的值1，要插入的值2),

   ​				(要插入的值1，要插入的值2);

#### 查询数据

```
SELECT [DISTINCT] {* | {column [, column] ...}  
[FROM table_name]
[WHERE ...] 
[ORDER BY column [ASC | DESC], ...] LIMIT ...
```

全列查询：查询该表所有字段、所有数据

select * from 表名；

![image-20230321002848881](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321002848881.png)

指定列查询：

from 查询列1，查询列2 from 表名;

![image-20230321002913520](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321002913520.png)

如果字段数数值型，可以使用运算符计算

select id,name,amount*2 from student;

![image-20230321003522746](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321003522746.png)

> 查询出来的结果集，并不等于数据库表中的那张原始二维表，
>
> 字符串：MySQL中，字符串拼接，不能使用加号，需要使用  ’concat(str1,str2.....);‘
>
> 表和字段别名：
>
> 字段：select 字段名 [as] 别名 from 表名；
>
> ![image-20230321004601982](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321004601982.png)
>
> 表：select ... form 表名 [as] 别名
>
> ![image-20230321005006840](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321005006840.png)
>
> 表使用了别名，字段一定要加表别名，如：表别名.字段名
>
> 

日期操作，使用函数

#### 去重 distinct

select distinct 字段名 from 表名

> 当有多个字段时，会对所有字段综合性判断，所有字段均重复的去掉，有个别非重复字段的，不去重，如下图，仅去重张三

![image-20230321011638375](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321011638375.png)

![image-20230321011736566](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321011736566.png)

#### 排序  order by

asc升序  desc降序  （默认为升序）

```mysql
select 字段 from 表名 order by 希望排序的字段 排序方式  
///order by 字段1 [asc|desc],字段2 [asc|desc]   多字段排序顺序按字段顺序，先排字段1，在排字段2，以此类推
select id，name form student order by id asc；
```

> 排序也可以使用表达式及别名

#### 条件查询

select 字段 form 表名 where 条件；

![image-20230321103119588](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321103119588.png)

> null:不能使用比较运算符，只能使用is/is not null

#### 范围查询 

between ... and ...          字段名 between 起始值 and 结束值；

> and 优先级高于 or

![image-20230321104244296](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321104244296.png)

![image-20230321104258940](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321104258940.png)



in （多个值）   先遍历所有数据，返回true

![image-20230321104816878](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321104816878.png)



#### like 模糊匹配

% 匹配 任意多个字符

_  匹配 一个字符

![image-20230321105520995](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321105520995.png)

![image-20230321105534870](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321105534870.png)

#### 分页查询 limit

一般来说，一个查询结果集是确定的，给定每页的数量进行分页，就可以确定页数

数据库中华是按照起始的索引+每页的数量

```mysql
SELECT ... FROM table_name [WHERE ...] [ORDER BY ...] LIMIT n OFFSET s;  // 从 s 开始，筛选 n 条结果，(每行数据下标从0开始)
```

![image-20230321124554114](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321124554114.png)

#### 修改

update 表名 set 字段1 = 要修改的值 ， 字段2= ... where 条件 order by  

> update修改操作可以对多行进行修改，若没有添加条件，则修改所欲值

![image-20230321131316686](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321131316686.png)

![image-20230321131325117](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321131325117.png)

#### 删除 delete

delete from 表名 where 条件 order by...

### 增删改查进阶

#### 数据库约束

表定义的时候，某些字段保存的数据，需要按照一定的约束条件

#### null约束

字段null：该字段可以为空（默认）

字段 not null：该字段不能为空

#### unique唯一约束

表示某个字段不能重复

#### default默认值约束

default ’ 默认值‘ 

某字段设置default及默认值，插入时该列未插入，则自动插入默认值

> 显示插入null，默认值也不会生效

#### primary key 主键约束

主键约束（Primary Key Constraint）用于确保表中的每一行都具有唯一标识符，以便能够准确地引用和操作表中的特定行。主键约束通常是在创建表时定义的，并指定一个或多个列作为主键。这些列的值必须是唯一的，并且不能为NULL。当试图插入重复的主键值或NULL值时，数据库会拒绝该操作并返回错误。

> 当一个表具有主键约束时，它将确保该表中的每一行都具有唯一标识符。这个唯一标识符是由主键列的值组成的。例如，假设我们有一个名为“students”的表，其中包含以下列：ID、Name和Age。我们可以将“ID”列指定为主键，这将确保在表中不会有两行具有相同的“ID”值。当试图插入一行具有重复的“ID”值时，主键约束将阻止该操作，并返回一个错误消息。这可以防止数据重复或不一致，因为每个行都必须具有唯一的标识符。
>
> 此外，主键约束还可以帮助加快查询操作。因为主键列的值是唯一的，所以可以使用主键来快速查找和检索特定行，而不必扫描整个表。总之，主键约束是确保数据库表中数据完整性和一致性

1. 主键值必须唯一：每行的主键值必须是唯一的，否则就会违反主键约束。
2. 主键值不能为空：主键值必须非NULL，因为NULL值无法唯一地标识行。*//unique 可以多行null，因此不能唯一地标识*
3. 一个表只能有一个主键：一个表只能有一个主键，因为每个表只能有一种方式来唯一地标识行。

```mysql
primary key = not null unique 
```

![image-20230321175457764](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321175457764.png)

对于整数类型的主键，常配搭自增长==auto_increment==来使用。插入数据对应字段不给值时，使用==最大值+1==。//mysql关键字

#### foreign key 外键约束

用于设计表与表之间地关系 ：表1（主表：主键）——表2（从表：外键）从而建立表1与表2，一对一或一对多地关系。

```
foreign key (字段名) references 主表(列)
```



#### check约束

check （sex = ’男‘ or sex = ‘女’ ）

# 4、表的设计

三大范式：一对一，一对多，多对多

多对多：两张主表建立多对多关系，这个多对多关系，在两张主表中没有外键体现，使用一张单独的中间表，来表示两张主表的多对多关系

![image-20230321184150450](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230321184150450.png)

中间表：两个外键分别关联两张主表的两个主键，以及一些其他业务。

### 建表

主表地主键  	关联 	从表的外键	

```mysql
-- 学生表
DROP TABLE IF EXISTS student;
CREATE TABLE student(
    id int PRIMARY key auto_increment,
    name VARCHAR(20) not null
);


-- 课程表
DROP TABLE IF EXISTS course;
CREATE TABLE course(
    id int PRIMARY key auto_increment,
    name VARCHAR(20) not null
);
```

```mysql
-- 成绩表
DROP TABLE IF EXISTS exam_course;
CREATE TABLE exam_course(
    id int PRIMARY key auto_increment,
    -- 用学生表主键作为外键
    student_id int,
    course_id int,
    -- 建立主外键关联
    foreign key (student_id) references student(id),
    foreign key (course_id) references course(id)
);
```

**学生表与课程表之间的多对多关系，用过成绩表来体现**

课程表与学生表分别与成绩表之间建立一对多关系

#### 增删查改进阶

插入：insert into 表名 select ... form 表  where  ...  order  by ...  limit ...

将查询到的数据插入到表中

# 5、查询

### 5.1聚合查询

#### 5.1.1 聚合查询函数 （结果集聚合，合并为一行）

count（）	获取整个结果集行数（数据的数量）

sun（某个字段）	将结果集某字段求和计算

avg（某个字段）	将结果集某字段求平均值

max（某个字段）	将结果集某字段求最大值

min（某个字段）	将结果集某字段求最小值

> 聚合发生在查询返回结果集后，再进行聚合

![image-20230322132412279](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322132412279.png)

#### 5.1.2 分组

group by 分组	

```mysql
select 查询字段 	form	 表	where	条件	group 	by	分组字段1，分组字段2
```



> 1. GROUP BY子句中的列名必须出现在SELECT语句中，否则会出现语法错误。
> 2. 如果在SELECT语句中使用聚合函数，比如SUM、AVG、COUNT等，则必须使用GROUP BY子句将查询结果分组。
> 3. 在GROUP BY子句中可以使用多个列名，这样就会按照这些列的组合进行分组。
> 4. 如果使用了聚合函数，则可以使用HAVING子句对分组后的结果进行过滤。



having条件过滤

分组后的过滤不能使用`where`,而要使用having

### 5.2 联合查询（多表查询、复合查询）

==两张表的笛卡尔积==，

使得两张表的每条数据相连接，产生一个结果集，对笛卡尔积产生的结果进行过滤（去掉不符合的、没有意义的数据）

select 	*	form 	表1，表2；	//多张表之间逗号间隔，即返回笛卡尔积

![image-20230322151536561](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322151536561.png)

![image-20230322151548084](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322151548084.png)

使用字段时必须 ==表名.字段名==，可以使用别名，

![image-20230322151939091](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322151939091.png)

### 建立链接关系的方式

### 5.2.1内连接

```mysql
select 查询字段 from 表1，表2	where 链接条件	and 其他条件
select 查询字段 from 表1 join 表2	on 链接条件	and 其他条件
```

一种用于在两个或多个表中共享匹配行的SQL JOIN操作。内连接仅返回两个表中都存在的匹配行。使用内连接，您可以将一张表中的行与另一张表中的行相匹配。如果两个表中的某些行满足特定的条件，则它们将被返回为查询结果。

==必须满足连接条件+其他条件才会返回==

![image-20230322163852241](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322163852241.png)

### 5.2.2 外连接

只能使用 `join on`的形式，且 `lift join`是左连接，`right on`是外连接。

```mysql
select 查询字段 from 左表 lift join 右表 on 链接条件 and 其他条件
select 查询字段 from 左表 right join 右表 on 链接条件 and 其他条件
```

==满足连接条件+其他条件，或满足其他条件，就会返回==

> 左右连接的判断是，哪个表是完全包含的，，即左表和右表谁是外表，是左表就是左连接，右表就是右链接
>
> 在左外连接中，左表是完全包含的，而右表只包含符合条件的数据。具体来说，左表中所有的数据都会被保留，而右表中只有符合条件的数据会被保留，未符合条件的数据会被填充为 NULL。
>
> 在右外连接中，右表是完全包含的，而左表只包含符合条件的数据。具体来说，右表中所有的数据都会被保留，而左表中只有符合条件的数据会被保留，未符合条件的数据会被填充为 NULL。

![image-20230322165530872](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322165530872.png)

注意使用其他条件时，on后是连接条件，where后是其他条件（where后的必须保证）

![image-20230322170008014](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322170008014.png)

### 5.2.3 自连接

表自己连接自己，常用于自身多行数据统一字段的比较(同一个表中)

![image-20230322202154419](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322202154419.png)

### 5.2.4 子查询

嵌入在其他sql语句中的select查询，也叫嵌套查询

![image-20230322202945466](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230322202945466.png)

##### 多行子查询

[not]	in（多个常量值，逗号间隔）返回多行数据

一个字段	in	（子查询） 返回多行数据，一个字段

（多个字段）in （子查询）返回多行数据，查询字段和in前面的字段顺序相同

```mysql
select * from score where course_id in (select id from course where name='语文' or name='英文');
```

[not] 	exist

```mysql
select * from exam_score e where exist( select	1 form student s 
                                       where 
                                       e.sutdent_id = s.id 
                                       and (s.name ='张三' or s.name = '李四' ));
```

> 在from子句中使用子查询：子查询语句出现在from子句中。这里要用到数据查询的技巧，把一个 子查询当做一个临时表使用。

### 5.3 合并查询

在实际应用中，为了合并多个select的执行结果，可以使用集合操作符 union，union all。使用UNION 和UNION ALL时，前后查询的结果集中，字段需要一致。

union：取结果集并集，自动去除重复行

union all：取结果集并集

# 6、索引和事务

### 索引

索引：使用一定的数据结构，来保存字段对应的数据，以根据索引字段来检索，提高检索效率。

> 要考虑对数据库表的某列或某几列创建索引，需要考虑以下几点： 
>
> 数据量较大，且经常对这些列进行条件查询。 
>
> 该数据库表的插入操作，及对这些列的修改操作频率较低。 索引会占用额外的磁盘空间。 
>
> 满足以上条件时，考虑对表中的这些字段创建索引，以提高查询效率。 

反之，如果非条件查询列，或经常做插入、修改操作，或磁盘空间不足时，不考虑创建索引。

```mysql
create index 索引名 on 表名(字段名);
```

B树：所有节点都保存有所有及数据（数据、page指针），搜索路径，从根节点搜索到叶子节点，相比链表查询效率高

B+树：仅叶子节点保存有索引例和数据，非叶子节点只有索引字段（键值和page指针）

选则B+原因：

1. 节点不储存data，这样一个节点可以储存更多的key，可以使得树更矮，IO操作次数更少，
2. 叶子节点相连，便于进行范围查找

------

一个索引包含所欲要查询字段的值，称之为索引覆盖

select	覆盖索引字段	from	表	where	覆盖索引字段 = ...

```
select name form table	where	name = '' ;
```

> 检索的时候根据name字段来检索（name在叶子节点、排序的），查询字段又没有其他字段，就不需要回表



基于B+树的索引

1、主键索引（聚簇索引）

2、非聚簇索引

优化原则：

1. 索引字段尽量不要又null值。
2. 频繁查询的字段建立索引：需要考虑左匹配原则，顺序是左边
3. 频繁更新的字段，慎用（构建索引、插入、删除都需要一定是时间和计算资源）

[hash索引（hash表）](https://heapdump.cn/article/4692338)

> hashmap:
>
> 存储数据的特性：键值对，无序，建唯一（不重复）
>
> 底层数据结构：数组+链表+红黑树（数组可以通过下标访问，实现快速查询，链表用来解决哈希冲突）
>
> ==原理==：存取元素，复杂的都是put流程
>
> 1. 计算数组位置（根据key的hashcode值，基于内部hash函数计算出数组索引）
>
> 2. 判断数组是否为空，如果为空，就执行扩容，初始化数据大小。
>
>    如果数组不为空，根据哈希值找到数组所在下标
>
> 3. 判断下标元素是否为null，如果为null就创建新元素
>
> 4. 如果下标元素不为null，就判断是否是红黑树类型，如果是，则执行红黑树的新增逻辑
>
> 5. 如果不是红黑树，说明是链表，就追加到链表末尾
>
> 6. 如果判断链表长度是否大于等于8，数组长度是否大于等于64，如果不是就执行扩容逻辑
>
> 7. 如果是，则需要把链表转换成红黑树
>
> 8. 最后判断新增元素后，判断元素个数是否大于阈值（16*0.75=12），如果是则执行扩容逻辑，结束。

1. 定位快
2. hash冲突
3. hash索引不支持顺序和范围查询

### 事务

事务指逻辑上的一组操作，组成这组操作的各个单元，要么全部成功，要么全部失败。

在不同的环境中，都可以有事务。对应在数据库中，就是数据库事务。

> 事务的特性（ACID）
>
> - Atomicity（原子性）：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被恢复（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
>
>   > 多条sql要么都执行，要么都不执行
>
> - Consistency（一致性）：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
>
>   > 整个业务操作前后，保持一致
>
> - Isolation（隔离性）：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
>
>   > 不同的隔离级别下，不一定满足某种场景的隔离性
>
> - Durability（持久性）：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失
>
>   > 开启事务以后，sql的相关操作都是在内存中执行的，事务提交后，数据持久化到硬盘

