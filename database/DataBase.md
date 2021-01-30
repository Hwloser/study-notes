## 数据

### 1. 结构化数据

所有的数据结构都相同

### 2. 半结构化数据

xml，数据的结构大致相同，但可以扩增或减少等

点击流，

### 3. 无结构化数据

音视频，图片

### 

类型：

1. table

一行称之为元组，一列称之为属性。

2. view

逻辑映射

3. index
4. Program unit

## sql语句的分类

* Data retrieve
	* SELECT
* Data manipulation language
	* 
* Data definition 
	*
* 

## mysql系统结构

用户表和系统表

## mysql的配置文件

## 查看系统变量

### show variables

### show variables like '模糊匹配' \G ;

_表示任意一个字符

%表示0到多个字符

注意：字符串和时间在数据库中表示都要加单引号。

### 设置mysql的编码以及编码集

/etc/mysql/mysql.conf.d/mysqld.cnf

[client]
default-character
[mysqld]
character-set-setver=utf8
collation-server=utf8_general_ci

### mysql -uroot -p123456 -hipaddress -pport

### 展示数据库

show databases;

### 使用具体的数据库

use databaseName；

### 展示数据库中的所有表

show tables [like 'eg%']

eg通配

show tables from mysql like 'u%';

show tables [form db_name like '模糊查询']

### 创建自己的数据库

create database DBname;

### 显示数据库中表的结构

desc [db_name.]table_name;

e.g. desc user;

或
show columns from table_name [from db_name like ''];

### 列出建表语句

show create table table_name; 

### 列出表中的索引

show index from table_name;
e.g.:
show index from user;

### 列出某个数据库中的索引

show index from table_name from db_name;

### 列出mysql中运行中的状态信息

show status;

### 环境变量

show variables;

### 显示当前有多少线程连入(如果不加full，只会显示前100个)

show full processlist;

### 列出db_name中table_name表中列的状态信息

show table status from db_name;

### 查看某个user的权限

show grants for user@'ip';

### WITH GRANT OPTION：表示运行grant传递下去。

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION

### 列出数据库中所有引擎(引擎：一段代码，维护数据的录入，查询，数据库的安全，事务的支持，联机处理或联机分析)

show engines;

一般默认的引擎有三种：MyISAM、Memory、InnoDB

## select

### 直接做运算

select 1+1；

### 查看当前时间

select now();(实时)

select current_date;(查看当前日期)

select sysdate() as alias;(查看系统时间)
注意：as后面跟别名

## 数据类型

* 
1. tinyint(m)	1字节	8bit
2. smallint		2字节	
3. mediumint	4字节
4. int			8字节
5. integer		int的同义词
6. Bigint		

* 1. float(M,D)		2字节；
M:有效位数	D小数点后几位(会四舍五入)

* char

varchar(M):存储可变长度的字符串
M表示对可变长度的最大长度的限制

* tinyblob和tinytext一样；存储长度都为255

* blob 64k 1 HEX将一个字符或数字转换为十六进制格式
一般情况下可以存储图片、视频、音频、etc
* 使用UNHEX()

* TEXT	65535字符
* MEDIUMBLOB	存储16M
* MEDIUMTEXT	存储。。。字符
* LONGBLOB	存储4G
* LONGTEXT	存储

### 枚举类型

enum('val','val1')	枚举类型能且只能从指定的值中取一个值
最多可以设置为65535个

set('val1','val2'...)	一个集合，可以存放多个值
e.g.	插入多个值	insert into test values('male,female');//null

### 日期和时间类型

Date	日期格式yyyy-mm-dd

insert into test values(1,'2019-09-11')

DateTime	YYYY-MM-DD HH:MM:SS

TimeStamp(M)	一个时间戳记；M表示显示格式的长度
java.sql.Timestamp
M=>14->YYYYMMDDHHMMSS
M=>12->YYMMDDHHMMSS
M=>8->YYYYMMDD
M=>6->YYMMDD

time	HH:MM:SS

year[2|4]	
2->YY
4->YYYY

## 插入值

insert into table_name() values();

## 创建表

create table table_name(
column_name float(M,D) zerofill
);

zerofill：用0填充空余的小数位

char(M)：M表示长度：固定长度的存储方式
数据库存储长度不够会使用空格补齐
char(M)一般会申明固定长度的值

汉字需要用''单引号

binary	二进制方式存储

## 删除表

drop table table_name;

# DDL语句

表：存储数据

## 建表语句语法

create [temporary] table [if not exties] table_name[(columns)] [table option] engine=innodb default charset=utf8;

temporary	临时表，与一般用于存储计算结果
if not exits	如果表存在就不创建，表存在的时候不会保存，否则ERROR 1050 (42S01): Table 'test' already exists
engine=innodb	设置表的默认引擎，如果有默认配置，则会直接以配置文件的配置进行配置
default charset=utf8	设置表的编码，查看环境变量如果系统变量设置过编码，建表的时候不需要设置

create table table_name(
columns_name	type	constaints,
columns_name	type	constaints,
columns_name	type	constaints
);
注意：约束是对列的限制：
primary key：主键约束
not null：非空约束，指定的列值不能为空
unique：唯一约束，指定的列值的值必须唯一
foreign key：外键约束，不能用于列级约束申明
check-》mysql没有效果

表级约束
create table table_name(
columns_name	type,
columns_name	type,
columns_name	type,
constraint，
constraint，
constraint
);
注意：列级约束和表级约束可以混合使用

1. primary key:能够唯一标识一行内容的列，非空且唯一
2. 列级约束
create table test( 
id int primary key, 
name varchar(20), 
age tinyint unsigned , 
gender enum('男','女')  
);
3. 表级约束
create table test( 
id int, 
name varchar(20), 
age tinyint unsigned , 
gender enum('男','女')
primary key（id）  
);
4. 联合主键：表中任一一列都不能标识一行数据，可以选择多列作为联合唯一一行记录
5. 注意：联合主键只能用表级约束
create table test( 
id int, 
name varchar(20), 
age tinyint unsigned , 
gender enum('男','女')
primary key（id，name）  
);
联合主键：主键的组合不能重复，而联合主键中的单独的一个可以重复

1. 非空约束：
create table test( 
id int, 
name varchar(20) not null, 
age tinyint unsigned , 
gender enum('男','女') 
);
2. not null只能用于列级约束

1. unique：唯一约束，要求录入列中的数据不能重复
create table test( 
id int, 
name varchar(20) unique,  
age tinyint unsigned , 
gender enum('男','女')  
);
2. null可以重复
3. 表级约束==》
create table test( 
id int, 
name varchar(20), 
age tinyint unsigned , 
gender enum('男','女') ,
unique Unique_name(name)
);
4. 可以给unique起一个别名

1. 联合唯一：
create table test( 
id int, 
name varchar(20), 
age tinyint unsigned , 
gender enum('男','女') ,
unique Unique_name(id,name)
);

1. 自增属性，一般和主键连用(此时主键是int)
2. 自增，默认值1，步长为1
create table test( 
id int auto_increment primary key,
name varchar(20) 
);
3. 注意auto_increment修饰的列插入数据的时候可以忽略
 
会话：session	
对于数据库的一次连接或jdbc程序连入数据库获取连接对象
会话结束：终端连接或程序结束关闭连接对象

会话级（只针对当前终端）
show variables like '%increment%';	
查询auto_increment_offset默认初始值
和auto_increment_increment默认步长

1. 设置默认自增初始值
create table test( 
id int(14) auto_increment , 
name varchar(20) ,
primary key(id)
)auto_increment=10;

1. 外键约束：foreign key：
2. 维护表于表之间关系
3. 外键对应的列一定引用于另外一张表的主键或唯一约束的列
4. 在mysql中，外键必须用表级约束
5. 表于表之间是单边维护的关系
6. 一对一外键随便建在哪一方都可以
7. 外键类的类型于
create table t1( 
id int(4) auto_increment, 
name varchar(20), 
primary key(id) 
);
create table t2( 
id int(4) auto_increment, 
name varchar(20), 
primary key(id) ，
t1_id int(4) references t1(id)	//mysql中不支持，oracle支持
);

**mysql支持的：**
create table t1( 
id int(4) auto_increment, 
name varchar(20), 
primary key(id) ，
t2_id int(4),
foreign key(t2_id) references t2(id)
);

8. 联合外键
create table t1( 
id int(4) auto_increment, 
name varchar(20), 
primary key(id) 
);
create table t2( 
id int(4) auto_increment, 
name varchar(20), 
primary key(id) ，
t1_id	int(4)	,
t1_name	varchar(20),
foreign key(t1_id,t1_name) references t1(id,name)	
);

default给表中某列设置默认值：用户录入数据以用户录入的为主，没有录入则采用默认值

create table t1(
id int,
name varchar(20) default 'huanwei',
brith date default '2019-09-11',
age int default 20
);

## 数据库删除数据

delete from table_name;

## 数据库创建索引

索引一般不用建立，什么情况需要创建索引
1. 表中的数据基本不会变动的时候可以手动创建索引
2. 表中某一列常常作为查询条件
3. 每次查询的结果数据是总数据的4%左右

create table t1(
id int,
name varchar(20),
index index_key(columns_name)
);
==》
create table t1(
id int,
name varchar(20),
key index_key(columns_name)
);
==》create table t1(
id int key,
name varchar(20),
);

给约束起名字
列级
加载约束的最开始部分：constraint constraint_name
create table t1(
id int constraint t1_pk_id primary key ,
name varchar(20) constraint  not null nn,
number int unique
);
表级
create table t1(
id int  ,
name varchar(20) ,
number int unique,
constraint t1_pk_id primary key(id),
constraint nn unique(name)
);

1. 
check检查约束：限定录入数据值的范围(mysql不支持)
create table t1(
id int  ,
name varchar(20) ,
number int unique,
gender varchar(20) check(gender='男' or gender='女')
age int check(age between 0 and 20)		//闭区间
);

## ALTER

表结构的修改：
alter table table_name add [column ]
column_name column_name_type constrains [after|first 已存在的列名];

1. add 
	1. unique [index_name] un_index(column_name);
	2. unique(column_name);

2. 设置默认值
	1. alter table table_name alter colimn_name set default value;
	2. 默认值如果是字符串和date使用''
	3. 删除默认值
		1. ALTER  TABLE  table_name   ALTER  字段名称   DROP   DEFAULT

3. alter table t1 change id id int auto_increment;
	1. alter table table_name change old_field_name new_field_name constraint
4. alter table t1 modify id int(20);
	1. alter table table_name change field_name type constraint;
5. alter table t1 drop field_name;
6. alter table t1 drop primary key;
7. 删除索引的名字
	1. alter table t1 drop Index index_name;
8. alter table table_old_name rename table_new_name

## DML操作

1. insert
	1. insert low_priority `|` delayed into table_name [column_name,...] values(val,..)
	2. low_priority `|` delayed执行插入操作的时候，如果有其他的终端在操作数据，等其他终端操作完成再执行

	1. mysql中可以同时插入多行数据
	2. insert into table_name values(val..),(val...);
	3. insert into table1_name select field_name,... from table2_name;
	4. 注意：table1中的field_name要与table2中的field相同

2. delete

	1. [low_priority] from table_name where condition;
	2. delete 是只删除数据，不删除数据占据的磁盘空间。
	3. truncate 截取表，把表中的数据以及数据所占的空间全部收回
		1. truncate table_name;
	4. delete from table_name；清空表中的所有数据
	5. 加where condition，删除符合condition的数据
	6. delete from table_name limit n;
	7. 删除前n条语句
	8. 

3. update
	1. 修改表中的数据
	1. update table_name set column_name=xx,condition...;
	2. update table_name set column_name=xx,condition... limit n;
	3. update table_name set column_name=xx,condition... where condition limit n;
	4. 不加where 将全表修改

4. optimize
	1. 优化表（整理表）
	2. optimize table table_name;

## 操作符和函数

### 算术操作符

1. `+ - *`
2. `/ div`除法
3. `mod(被除数，除数) %`取余
2. `| & << >> ^ ~ !`
3. `< <= > >= `
4. `!=  <>`

select column_name alias_name,... from table_name where condition;

### 逻辑操作符

1. expression1 or expression2 -》 或
	1. where field_name in(expression1,expression2,expression3) -》 或
	2. where field_name not in(expression1,expression2,expression3) -》 


2. expression1 and expression2 -》 与
3. not expression1 -》 非

1. between and -》 且是闭区间
2. not between and
	1. 注意：and表示并且，前后都要成立，数据会保留

1. case when condition_expression then result_expression 
2. else expression 
3. end alias_name,next_select_field
	1. 多个条件判断语句	
	2. e.g.
	3. select id,case when last_name='Patel' then 'ok' 
	4. when last_name='Smith' then 'no bye' 
	5. else 'go go..' 
	6. end name,salary
	7. from s_emp;

1. exists subquery
2. exists用于检查子查询是否至少会返回一行数据，该子查询实际上并不返回任何数据，而是返回值True或False。
exists指定一个子查询，检测行的存在。
	1. select id,last_name,first_name 
	2. from s_emp 
	3. where exists(select id,last_name from s_emp where last_name='Smith');

1. is null	-》	为空
2. is not null	-》	不为空

1. 模糊查询
2. like 模糊查询
3. e.g.：field_name=''...;

### 函数

字符函数

1. LOWER 转换大小写
2. select lower('huanwei')
3. from table_name;

1. concat 拼接字符串
2. select concat('huanwei','nihao');

1. SUBSTR 截取字符串
2. select substr('huanwei',2,2);
3. 两个参数，
4. 第二个参数表示从第几个开始截取
4. 第三个表示截取的位数

1. LENGTH 获取字符串的长度
2. select 	length('huanwei');

数字函数

1. round 四舍五入
	1. select round(1.524);
	2. select round(1.523,2);//1.235
	3. select round(123.23456,-1);//120
	4. select round(3.23456,-1);//0
	5. 第二个参数表示小数点后保留几位
2. mod 取余
	1. 

日期函数

1. select curdate();//yyyy-mm-dd
2. select curtime();//HH:MM;SS
3. select now();//yyyy-mm-dd HH:MM:SS
4. ... subdate(date,x);表示距离date的**前**x天的yyyy-mm-dd
	1. select subdate(curdate(),1);
	2. select subdate('2019-08-23',1);
	3. select subdate('2019-08-23',-1);
5. ... adddate(date,x):表示距离date的**后**x天的yyyy-mm-dd
6. subtime()指定时间的**前**几秒
	1. e.g.
	2. select subtime(curtime(),-59);
7. addtime()指定时间的**后**几秒
8. datediff(time2,time1)两个时间相差多少天
	1. select datediff(subdate(curdate(),1),adddate(curdate(),1));//time1-time2;
9. last_day(time)//查看指定日期的当前月份的最后一天
	1. select last_day('2019-9-17');
10. year(now());//获取指定时间的年份
11. week(now());//获取指定时间的周
12. dayofmonth(now());
13. day(now());
14. quarter(now());//查看当前季度
15. hours(now());
16. minute(now());
17. second(now());
18. date_format()//将时间转化为特定格式输出
	1. select date_format(now(),'%b %D %d %Y');
19. str_to_date()//将字符串格式的字符串
	1. select str_to_date('20190917','%Y %m %d');//2019-09-17

其他函数(对字符串进行加密和解密)
一、
1. AES_ENCRYPT(key1,key)
	1. 第一个参数是需要加密的字符串
	2. 第二个参数是密钥
2. AES_DECRYPT(key1,key)
	1. 第一个参数是需要解密的字符串
	2. 第二个参数是密钥
二、
1. encode(key1,key)
	1. 第一个参数是需要加密的字符串
	2. 第二个参数是密钥
2. decode(key1,key)
	1. 第一个参数是需要解密的字符串
	2. 第二个参数是密钥

三、不可逆
1. MD5(str);
	1. str需要加密的字符串
	2. 产生32位的字符串
四、
1. ENCRYPT(str)
	1. 在windows下不可行
	2. ENCRYPT采用的算法是linux内置的

五、
1. PASSWORD(str)
	1. 数据库支持

## select 查询语法

select [SQL_SMALL_RESULT`|`SQL_BIG_RESULT`|`HIGH_PRIORITY`|`distinct] column1_name [as] alias_name,column2_name....
from table_name_reference
where condition(对表中查询的结果逐行进行筛选)
groupby 分组(把相同列的值归为一个组)
having 条件：对分组之后的结果进行逐行的筛选
order by 对展示的数据进行排序
limit 限制展示的函数
 
注意：
1. SQL_SMALL_RESULT 查询的结果比较小，from查询出来的数据用临时表存储
2. SQL_BIG_RESULT 查询的结果比较大，可以用磁盘存储
3. HIGH_PRIORITY 优先级为HIGH
4. 执行顺序 from->where->groupby->order by->having->limit

1. distinct 去重操作
	1. distinct只能在select之后
	2. distinct去重是对select之后的column组合进行去重
2. order by field1_name,field2_name arg(排序)
	1. arg:following	arg可以不加会以默认排序进行
	1. ASC 默认升序排序
	2. DESC 降序
	3. e.g.
	4. select salary  
	5. from s_emp 
	6. order by salary desc;
	7. order by后跟多列，先按照第一列排序，第一列排序相等的按照第二列排序，以此类推
	8. order by后面的列可以用数字表示
	9. select salary,last_name  
	10. from s_emp 
	11. order by 2;
	12. 其中2表示的是last_name的位置
3. 多表查询
	1. select table_name1.field_name1...table2_name.filed_name2
	2. from table_1 alias_name1,table2 alias_name2;
	3. 多表中给表起别名
4. 等值链接(内链接)
	1. 从多表得到的笛卡尔积中基于关系找出具有关联关系的数据
	2. select s_dept.id,s_dept.name,s_region.id,s_region.name 
	3. from s_dept,s_region 
	4. where mod(s_dept.id,s_region.id)!=0;
5. straight_join强制顺序链接
	1. 由straight_join链接的多张表
	2. select s_dept.id,s_dept.name,s_region.id,s_region.name 
	3. from s_dept straight_join s_region 
	4. where mod(s_dept.id,s_region.id)!=0;
6. 不等值链接
	1. 两张表没有主外键关联关系
7. 自连接
	1. 表自身连接自身
8. group by后跟列，分组的标准(值相等的归为一组，分组之后的结果只能给出一行内容)
### 外连接

1. 左外链接-》
2. 需要在等值连接的基础上，连接前面的是主表，后面的是从表
	1. lefr outer join
	2. select t1.id,t1.name,t2.id,t2.name 
	3. from t1 left outer join t2 
	4. on t1.id=t2.id;

2. 右外链接
	1. select t1.id,t1.name,t2.id,t2.name 
	2. from t1 right outer join t2 
	3. on t1.id=t2.id;

3. 全连接
	1. 

### 子查询

1. 子查询(嵌套查询)
2. 一个查询是另一个查询的条件
	1. select salary  
	2. from s_emp where salary>( 
	3. select salary 
	4. from s_emp 
	5. where last_name='Smith'
	6. );
	7. 
3. 

###组函数

1. avg() 求平均值
2. max
3. min
4. stddev	标准差
5. sum	求和
6. count	求数量和

### 
1. select field1,filed2
2. from table_name
3. group by Field1,field2;
4. having condition

1. select中出现的字段必须在group by clause中出现，但是可以跟组函数(other field)
2. where对from进行过滤
3. having对group by的结果进行处理

### 视图

视图：视图是一个虚拟表，它由存储的查询结果构成，可以将它的输出看作是一张表，所谓视图就是提取一张或者多张表的数据生成一个映射，管理视图可以同样达到操作原表的效果，方便数据的管理以及安全操作，同时能够把一些重要的数据隐藏起来。

视图的特点：

1. 视图的列可以来自不同的表，是表的抽象和逻辑意义上建立新的关系
2. 视图是由基本表(实表)产生的表（虚表）
3. 视图的建立和删除不影响基本表
4. 对视图内容的更新(CRUD)直接影响基本表(仅限于简单视图)

syntax：
1. create [algorithm=算法] view view_name 
2. as 
3. [查询操作];

注意：
algorithm=merge/temptable/undefined

简单视图

## 触发器（事件三要素）
1. 事件源-》
	1. 会产生事件对象
2. 监听器
3. 事件

创建syntax：
create trigger trigger_name
before/after event(insert/update/delete[可以多写，用，间隔])
on table_name(监听的表)
for each row #该语句固定
begin
sql语句;
sql语句;
...
(触发语句)
end

注意：
delimiter # 定义#为结束符
2. 
begin

1. if (expression) 
2. then sth,
3. else sth.
3. end if

end

## 事务-》只保护数据

数据库事务transaction 正确执行的四个基本要素。ACID，原子性(Atomicity)、一致性（Correspondence）、隔离性(Isolation)、持久性(Durability)。

1. start transaction手动开启事务
2. 提交事务commit transaction
	1. 有些会直接自动commit
3. 回滚事务rollback transaction
	1. 可以使用 savepoint point_name;
	2. 来存储回滚点
	3. 使用rollback to point_name
	4. rollback to point_name会在commit的时候才会使用到，单独使用的时候会报错point_name is doesn`t exists。

事务的使用：insert，update，delete

## 隔离性级别

select @@global.tx_isolation

1. READ UNCOMMITED


## 数据库备份

1. 导出
mysqldump 。。。>file

-A全部
-B+database_name

2. 导入
source file.sql
mysql -uroot -ppassword  < file.sql

## 给用户授权

grant 权限 on 数据库.表 to 用户@ip identified by 密码 with grant option。

权限：all，select，update。。。

数据库.表：e.g.：`*.*`

ip

## 回收权限

revoke privileges;

##立刻生效

flush privileges;

## 查看用户操作权限

select user,host from mysql.user;

