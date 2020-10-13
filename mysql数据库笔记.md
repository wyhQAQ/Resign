一.数据库的好处

1.可持久化数据到本地

2.结构化查询

二.数据库常见概念

1DB：数据库

2.DBMS：数据库管理系统

3.SQL：结构化查询语言

三.数据库存储数据特点

1.数据存放到表中，然后表再放到库中

2.一个库可以有多个表，每张表具有唯一表名标识自己

3.表中有一个或多个列，列称为字段。

数据库基本特点

数据结构化 ，数据之间具有联系，面向整du个系统；数据的共享性高，冗zhi余度低，易扩充；数据独立性高。

四.常见的数据库管理系统

mysql，oracle，db2，sqlservice



###mysql启动和停止

方式一：net start 服务名

net stop 服务名

方式二：计算机--右击--管理--服务



###mysql服务的登录和退出

方式1：通过mysql再带客户端/只限于root

方法2：通过windows自带客户端

登录：mysql 【-h主机名 -P端口号】-u用户名 -p密码

退出：

exit或ctrl+c



###mysql的常见命令

1.查看当前所有的数据库：show databases；

2打开指定的库：use 库名；

3查看当前库的所有表：show tables；

4.查看其他库的所有表：show table from 库名;

5创建表 create table 表名(

​                 列名 列类型，

​                 列名 列类型，)；

6.查看表结构 desc 表名；

7查看mysql版本：登录到mysql服务器：select() version；没有登录到mysql  mysql --version/mysql--V



###mysql语法规范

​        1.不区分大小写，但建议关键字大写，表名，列名小写。

​        2.每条命令用；结尾

​        3.每条命令根据需要，可以进行缩进或换行

​        4.注释

​              单行注释：#注释文字

​              单行注释：-- （空格) 注释文字

​               多行注释：/*注释文字*/

​        5显示表结构   DESC table;

###sql语句

基础查询

#进阶1 基础查询
/*
SELECT 查询列表
FROM 表名；

特点：
1查询列表可以是：表中字段、常量、表达式、函数
2.查询的结果是虚拟的表格
*/
#1.查询表中的单个字段
SELECT last_name FROM employees;
#2.查询表中多个字段
select last_name,salary,email from employees;
#3.查询表中所有字段
SELECT * from employees;

#4.查询常量值
SELECT 100;
SELECT 'john';

#5.查询表达式
SELECT 100*99;

#6.查询函数

select 函数（实参列表）

#7起别名 
SELECT 100*99 as 结果;
SELECT 100*99  结果;
select last_name as 姓,first_name as 名 from employees;
select salary AS 'out put' FROM employees

#去重
SELECT  DISTINCT department_id FROM employees;

#9 加号:只有运算符功能
select last_name+first_name as 姓名 from employees;

select CONCAT(last_name,first_name,...) as 姓名 FROM employees;

select ifnull（commission_pct,0)  as 结果;

isnull（字段）如果是null返回true，不是返回false

#进阶二：条件查询

/*

select

查询列表

from

表名

where

条件

1.条件表达式筛选 < > =  !=等

2.按逻辑表达式筛选

逻辑运算符    && || ！ and or not

3.迷糊查询 like between and in is null;

*/

一 按条件表达式筛选

查询部门编号不等于90的员工名和部门编号

select  last_name,department_id from employees where department_id!=90;

二按逻辑表达式筛选

三模糊查询

1. LIKE'Mi%' 将搜索以字母 Mi开头的所有字符串（如 Michael）。

​    2. LIKE'%er' 将搜索以字母 er 结尾的所有字符串（如 Worker、Reader）。

​    3. LIKE'%en%' 将搜索在任何位置包含字母 en 的所有字符串（如 When、Green）。

like 一般和通配符一起使用，通配符：%任意多个字符/_任意单个字符

案例：查询员工名字中第三个字符为a，第五个为a的员工和工资

select     last_name，salary,    from    employees  where   last_name   like  '__e_a%';

案例：查询员工名字中第二个字符为_的员工

select     last_name，salary,    from    employees  where   last_name   like  '_$_%'  escape '$';

between and(包含临界值/临界值不能交换)

in(等价于等于)

查询员工工种编号是IT_PROG、AD_VPAD_PRES中的员工和编号

select     last_name,job_id from  employees   where  in('IT','','');

is null/is not null

=或<>不能判断null

查询没有奖金的员工名和奖金率

select   last_name,commission_ptc from   where   commission_ptc is null  ;

安全等于<=>(可读性差)

select   last_name,commission_ptc from   where   commission_ptc <=> null  ;

案例查询工资为12000的员工

select   last_name, commission_ptc  from   where   salary<=>12000  ;

#进阶三

排序查询

特点：asc是升序，desc降序，默认升序

/*

select * from  employees

order by 排序列表  asc|desc

支持单个字段，多个，表达式，函数，别名

*/

案例1：查询员工信息，要求从高到底

select   *   from employees order by salary  desc；

案例2：查询部门>=90员工信息，入职时间排序

select  * from employees where department_id >=90 order by  hiredate  asc;

案例3：按年薪高低显示信息/按表达式

select * ，salary*12*(1+ifnull(commission_ptc,0))  年薪

from   employees    order by  salary*12*(1+ifnull(commission_ptc,0)）    desc

案例4：按姓名长度排序/函数

select  length（last_name） 字节长度，last_name，salary  from  employees  order by

length（last_name）DESC

案例5：多个字段，先按工资，再按编号排序

select * from employees order by salary asc，employee_id desc;

#进阶4

常见函数

/*

1.隐藏了实现细节2.提高重用性

调用：select   函数（实参列表） [from 表]

特点：1.叫什么/干什么

分类：1.单行函数2.分组函数

一.字符函数

#length   湖片区参数值的字符个数

select  length('john');

#concat  拼接字符串

select concat(UPPER（last_names）,LOWER（first_name）) 姓名 from employees；

#UPPER,LOWER 大写小写

#4.substr，substring

select substr("abc",a, b)//截取a开始指定长度b

案例：姓名首字符大写，其他小写，用_拼接

select concat(UPPER(Substr(last_name,1,1)),'_',Lower(SUBSTR(last_name,2)))  output

#.instr 返回子串第一次出现的位置，找不到返回0

select instr('xxxabc',‘abc’)   as output

#.trim

select  trim('           123       ')   as  out put;

select  trim('a'     from ' aaa123aaaa ')   as  out put;

#lpad 用指定字符实现左填充指定长度

select lpad ('abc',10,'*')  as  output;

#rpad 用指定字符实现左填充指定长度

select rpad ('abc',10,'*')  as  output;

#replace 

select replace('abcabccccc','ab','cc')  as out put;

 */

#二.数学函数

#round

select    round（1.65）；

select    round(1.567,2);//保留两位

#ceil   向上取整，返回大于等该参数最小整数

#floor   向下取整 返回<=该参数最大整数

#truncate 截取

select  truncate(1.6888,1)//小数点后截取一位

#mod取余，和%

mod(a,b):a-a/b*b

#rand:获取随机数



三.日期函数

#now 返回当前系统日期和时间

#curdate 返回当前系统日期，不包含时间

#curtime 返回当前时间

#可以获取指定部分

select     year(now())  年;

select     year(hiredate)   年   from employees；

select      monthname(now())   月;

#str_to_data:将日期格式的字符转换成指定格式

str_to_data('9-13-1999','%c-d-%Y')

select * from employees where hiredate=str_to_data('4-3 1992','%c-%d %Y');

#date_format:将日期转换成字符

select Date_Format(now(),'%y年%m月%d日')  as out put

select last_name,Date_format(hiredate,'%m月/%d日  %y年')  入职时间

from employees 

where commission_pct is not null;

datediff:返回两个日期相差时间

四.其他函数

#version()

#database()

#user()

#password('字符')//加密

五.流程控制函数

#if 函数:if  else

select  if(10>5,'大',‘小’)；

select last_name,commission_pct,if(commission_pct is null,'没','有')  备注 from employees

#case 函数使用一：switch case

/*

case 判断的字段表达式

when 常量1  then  语句1；/值1；

else 要显示n语句n；

end

select    salary 原始工资,dapartment_id,

case dapartment_id

when 30 then salary*1.1

when 40 then salary*1.2

when 50 then salary*1.3

else salary 

end  as  新工资

from employees

*/

#case用法二：多重if

case 

when  条件1  then要显示的值1或语句;

when  条件2  then

。。。

else 要显示的n或语句n

end

案例：员工工资

如果工资大于20000，A；大于15000，B；大于10000C，否则D

select salary

case when salary>20000 then 'A'

case when salary>15000 then 'B'

case when salary>10000 then 'C'

else 'D'

end  as level

#分组函数

/*功能：

用作统计使用，又称聚合函数或统计函数或组函数

sum求和 avg 平均值 max最大值 min最小值 count 计算个数

特点：
1.sum，avg一半用于处理数值型

​    max，min，count可以处理任何类型。

2.以上分组函数都忽略null值。

3.可以和distinct搭配

4.count函数单独介绍

一般用count(*)统计行数

5.和分组函数一同查询的字段要求是groupby后的字段。

*/

#1.简单使用

select   sum(salary) from employees

select   avg(salary) from employees

select   max(salary) from employees

select    count(salary) from employees//个数

#2.参数支持哪些类型

sum，avg只能处理数值型数据

max，min，count能够处理所有数值

#3.是否忽略null值

sum，avg，max，min，count忽略null值

#4.和distinct搭配

select sum(distinct salary),sum(salary) from employees;

select count(disctinct salary),count(salary) from employees;

#count的详细介绍

select count(salary) from employees;

select count(*) from employees;//统计个数

select count(1) from employees；//统计个数，每一行加一列1.

效率：innodb下count(*)和count(1)效率差不多

MYISAM下count(*)效率高

#6.和分组函数一同查询的字段限制

例：查询员工表中最大入职时间和最小入职时间相差天数

select DATEIF(max(hiredate),min(hiredate))   diffrennce  from employees;

例：查询90部门的人数

count(*)  from  employees where department_id=90;

#进阶5

/*

select 分组函数，列（出现在group by后面）

from 表

where 删选条件

group by  分组的列表

order by 子句



特点：分组前筛选：数据源来自原始表，groupby前，where

​            分组后筛选：数据源是分组后的结果集 groupby后，having

1.分组函数做条件一定是在having中

2.能用分组前筛选优先分组前筛选

group by字句支持但字段分组，多字段分组，无序

可以添加排序

*/

#查询工种最高工资

select   job_id，max(salary)

from employees

group by job_id;

#查询每个位置的部门个数

select count(*),location_id

from departments,

group by location_id;

#添加筛选条件

#案例1：查询邮箱中包含a字符的，每个部门的平均工资.

select avg(salary),department

from employees

where email like '%a%'

group by department_id;

#案例二:有奖金的每个领导手下员工工资

select max(salary),manager_id

from employees

where comission_pct is not null

group by manager_id;

#添加复杂筛选条件

#查询哪个部门员工个数大于2

select count(*),department_id

from employees

group by department_id

Having count(*)>2;//添加分组后的筛选

#查询每个工种有奖金的员工的最高工资>12000的工种编号和员工工资

#查询每个工种有奖金员工最高工资

select max(salary),job_id

from employees

where commission_ptc is not null

group by job_id

having max(salary)>12000;

#案件 查询领导编号大于102的每个领导手下工资最低工资>5000编号和领导id

select min(salary),manager_id

from employees

where manage_id>102

group by manage_id

having min(salary)>5000;

#按表达式分组

##案例：按员工姓名长度分组，查询每一组员工个数，筛选大于5的组

select count(*),length(last_name)   len_name

from employees

group by length(last_name)

having count(*)>5;

#按多个字段分组

##查询每个部门每个工种的平均工资

select avg(salary),department_id,job_id

from employees

group by  job_id,department_id;

#添加排序

##查询每个部门每个工种的平均工资，按从高到低排序

select avg(salary),department_id,job_id

from employees

where department_id is not null

group by  job_id,department_id

Having avg  (salary>1000)

order by avg(salary)  desc;

###进阶6

连接查询

/*

又称多表查询，当查询的字段来自多个表时

笛卡尔乘积

分类：

sql92标准：仅支持内连接

sql99标准：支持内连接+外连接（不支持全外连接）+交叉连接

按功能

内连接：等值连接；非等值连接；自连接

外连接：左外连接；右外连接；全外连接

交叉连接

*/

92标准

#一.等值连接

/*

多表等值连接结果为交集部分

n表连接至少要n-1连接条件

多表顺序没有要求

可以搭配所有排序，组合，筛选等使用

*/

select Name,boyName

from beauty,boys

where beauty.boyfriend_id=boys.id;

#查询员工名和对应部门名

select last_name,departname_name

from employees,departments

where employees.'department_id'=departments.'department_id'

为表起别名,如果起了别名就不能用原来的表名。

#查询员工名、工种号、工种名

select last_name,employees.job_id,job_title

from employees as e,jobs as j

where e.job_id=j.job_id;

##可以加筛选？

select last_name,department_name

from employees e,departments d

where e.id=d.id

and e.commission.ptc is not null;

##可以加分组吗

##查询每个城市的部门个数

select count(*)  个数，city

from departments d,locations l

where d.location_id=l.location_id

group by city;

##查询有奖金的每个部门名和部门领导变化，最低工资

select department_name,d.manager_id,min(salary)

from departments d,employees e

where d.'department_id'=e.'departments_id'

group by department_name,d.manager_id;

##可不可以排序

案例查询每个工种工种名和员工个数，并按员工个数降序

select job_title,count(*)

from jobs j,employees e

where j.job_id=e.job_id

group by job_title

having count(*) desc;

##三表连接

查询员工名、部门名和所在城市



#2.非等值连接

#案例：查询员工工资和工资级别

select salary grade_level

from employees e,job grades g

where salary between  g.max_salary and g.min_salary

and g.'grade_level'='A'

#3.自连接

案例：员工名，和上级名称

select e.employee_id,e.last_name,m.employee_id,m.last_name

from employees e,employees m

where e.'manager_id'=m.'manager_id'

##二 sql 99语法

/*

select 查询列表

from 表1 别名[连接类型]

join 表2   别名

on  连接条件

where 筛选条件

group by 筛选条件

内连接：inner

外连接：

左外：left

右外：right

全外：full

交叉连接：cross

##### ##1.内连接

from 表1 别名[连接类型]

inner join 表2   别名

on  连接条件

where 筛选条件

group by 筛选条件

分类：

###### #1.等值连接

特点：添加排序、分组、筛选/inner可以省略/筛选条件放where后，连接放在on后面

inner join 连接和sql 92里等值连接效果相同。

案例1.select last_name,department_name

from employees e

INNER JOIN departments d

on e.'department_id'=d.'department_id';

案例2.select last_name,job_title

from employees    e

INNER JOIN jobs j

on  e.job_id=j.job_id

where e.last_name like '%e%'

 案例3.select  count(*),city

from locations l

Inner JOIN departments d

on l.departments_id=d.departments_id

group by city

Having count(*)>3;

案例4.#查询员工名、部门名、工种名，按部门降序

Select last_name,department_id,job_title

from employees e

inner join departments d on e.department_id=d.department_id

inner join jobs j on e.'job_id'=j.'job_id'

order by department_name DESC;

###### #2.非等值连接

#案例1 查询员工工资级别

select salary , grade_level

from   employees   e

join   job_grade   g

on  e.salary between g.'lowest_salary'  AND  g.'highest_salary';

#查询每个级别的个数大于2，并按级别降序

select  count(*),salary , grade_level

from   employees   e

join   job_grade   g

on  e.salary between g.'lowest_salary'  AND  g.'highest_salary';

group by grade_level;

Having count(*)>20

ORDER  BY grade_level DESC

###### #3.自连接

#查询员工名字，上级名字

select e.last_name,m.last_name

from employees e

INNER JOIN employees m 

 on e.monitor_id=m.employee_id;

##### #二.外连接

/*应用场景：用于查询一个表有一个没有的数据。

1.外连接的查询结果为主表中的所有记录

​    如果表中有匹配，显示匹配结果

​    没有，显示null

 2.左外连接，left join 左边是主表，右外连接，right join右边是主表。

3.全外连接=内连接的结果+表1中有但表2没有的+表2中有但表1没有的

*/

select  b.name , bo.*

from beauty b

left outer   join   boys   bo

on  b.'boyfriend_id'=bo.'id'

where bo.'id'   is   null;



select  b.name , bo.*

from boys bo

right outer   join   beauty  b

on  b.'boyfriend_id'=bo.'id'

where bo.'id'   is   null;



#案例：查询哪个部门没员工

select d.*,e.employees_id

from departments d

left Outer JOIN employees e

on  d.'department_id'=e.'department_id'

where e.'employee_id' is null;

###### #2.全外连接

select b.*,bo.*

from beauty b

full outer join boys b

on  b.'boyfriend_id'=bo.'id';

##### #三.交叉连接

select b.*,bo.*

from beauty b

cross join boys bo;

##### #sql 92与sql 99

功能：sql 99支持的多

 可读性：sql 99实现连接条件与筛选条件分离，可读性高。

练习：查询编号大于3的女神的男朋友信息，如果有列出详情，如果没有，使用null

select b.*,bo.*

from beauty b

left outer join boys bo

on b.'boyfriend_id'=bo.'id';

where b.'id'>3;

select city d.*

from locations l

left outer join departments d

on l.'location_id'=d."location_id"

where d.'department_id' is null;

select e.*,d.department_name

from department d

Left  join employees e

on d.'department_id'=e.'department_id'

where d.'department_name' in('IT','dep')

#### #进阶7 子查询

#### /*

含义：出现在其他语句中的select语句，称为子查询或内查询

外部的查询语句，称为主查询



分类：按子查询出现的位置：

​                   select后面仅支持标量子查询

​                  from后面支持表子查询

​                 where或having后面标量子查询/列子查询/行子查询

​                 /exists后面表子查询

按结果集的行列数不同：

​                  标量子查询（结果就只有一行一列）

​                   列子查询（结果只有一列多行）

​                    行子查询（结果集有一行多列）

​                    表子查询（结果一半为多行多列）

#### */

#### #一，where或having后面的子查询

标量子查询（结果就只有一行一列）

列子查询（结果只有一列多行）

特点：1.子查询放在小括号里

2.子查询一般放在条件的右侧

3.标量子查询，一般搭配单行操作符用

列子查询，一般搭配多行操作符使用

4.子查询的执行优先于主查询。

##### #标量子查询

##查询abel的工资

select salary

from employees

where last_name='abel'

2.查询员工信息，满足salary大于abel

select *

from employees

where salary>(select salary

from employees

where last_name='abel');

案例 返回job_id与141号员工相同，salart比143号员工多的员工

##1.select job_id

from employees

where employee_id=141;

##2.select salary

from employees

where 

employee_id=143;

##3.select last_name,job_id,salary

from employees

where job_id=(

select job_id

from employees

where employee_id=141;

)

and salary>(select salary

from employees

where 

employee_id=143;

);

案例：返回公司工资最少的员工

select min(salary)

from employees

select last_name,job_id,salary

from employees

where salary=(

select min(salary)

from employees

);

案例：查询最低工资大于50号部门的最低工资的部门id和最低工资

1.select min(salary)

from employees

where department_id=50;

select min(salary),department_id

from employees

group by department_id;

having min(salary)>(

select min(salary)

from employees

where department_id=50;

);

#非法使用标量子查询

子查询结果不是一行一列

##### #2.列子查询

返回location_id是1400或1700部门中的所有员工姓名

#案例1.查询location_id是1400或1700的部门编号。

select distinct department_id

from departments

where location_id=1400 or location_id=1700;

#2.查询员工姓名

select last_name

from employees 

where department_id in(

select distinct department_id

from departments

where location_id=1400 or location_id=1700;

);

#案例2：返回其他部门比job_id为‘IT_PROG'部门任一工资低的员工号，姓名，job_id和salary

select distinct salary

from employees

where job_id='IT_PROG';

select  last_name,employees_id,job_id,salary

from employees 

where salary <any(

select distinct salary

from employees

where job_id='IT_PROG';

)

and job_id<>'IT_PROG';

select  last_name,employees_id,job_id,salary

from employees 

where salary <(

select max(salary)

from employees

where job_id='IT_PROG';

)

and job_id<>'IT_PROG';

#案例2：返回其他部门比job_id为‘IT_PROG'部门所有工资低的员工号，姓名，job_id和salary

select  last_name,employees_id,job_id,salary           

from employees 

where salary <all(

select distinct salary

from employees

where job_id='IT_PROG';

)

and job_id<>'IT_PROG';

select  last_name,employees_id,job_id,salary

from employees 

where salary <(

select min(salary)

from employees

where job_id='IT_PROG';

)

and job_id<>'IT_PROG';

#3.行子查询

#案例：查询员工编号最小并且工资最高的员工信息

行子查询写法：

select *

from employees

where (employee_id,salary)=(

select min(employee_id),max(salary)

);

select min(employee_id)

from employees

select max(salary)

from employees

select *

from employees

where employee_id=(

select min(employee_id)

from employees

)

and  salary=(

select max(salary)

from employees

);

### #二.select后面的子查询

#案例:查询每个部门员工个数

select d.*,(*

*select count* 

from employees e

where e.department=d.'department_id')

from departments d;

外连接实现:

select d.*,count(e.employee_id) 人数
from departments d
LEFT OUTER  JOIN employees e
ON e.department_id=d.department_id
GROUP BY d.department_id;

#案例：查询工号102的部门名

select (

select department_name 

from departments d

where 

INNER JOIN employees e

on d.department_id=e.department_id

where e.employee_id=102) ;

#三 from后面

#案例：查询每个部门平均工资的等级

1.select avg(salary),department_id

from employees 

group by departments_id

select * 

from job_grade,

2.连接1结果集和job_grade

select  ag_d.*,g.grade.level

from (

select avg(salary) ag,department_id

from employees 

group by departments_id

) as ag_d

inner join job_grade g

on  ag_d.ag between  lowest_sal and  highest_sal;

#四.exists后面（相关子查询）

/*

exists(完整的查询语句)

结果：1/0

*/

select exists(Select employee_id from employees);

案例1：查询有员工的部门名

select department_name

from departments

where exists(

select *

from employees e

where d.department_id=e.department_id;

);

select department_name
from departments d
where d.department_id in(
select department_id
from employees)

案例2：查询没有女朋友的男生信息

select bo.*

from boys vo

where bo.id not in(

select boyfriend_id

from beauty);



select bo.*

from boys bo

where not exists(

select   boyfriend_id

from     beauty   b

where bo.'id'=beauty.'boyfriend_id'

);

练习

1.查询和zlotkey相同部门的员工姓名和工资

SELECT last_name,salary
FROM employees
where department_id =(
select department_id
FROM employees
WHERE last_name='zlotkey'
);

2.查询工资比公司平均工资高的员工号，姓名和工资

SELECT employee_id,last_name,salary
FROM employees
WHERE salary>(
select AVG(salary)
FROM employees
)
ORDER BY salary DESC;

3.查询各部门中工资比本部门平均工资高的员工号和姓名

SELECT employee_id,last_name,salary,e.department_id
FROM employees e
INNER JOIN (SELECT avg(salary) s,department_id
FROM employees
GROUP BY department_id
) avg_d
ON e.department_id=avg_d.department_id
and e.salary>avg_d.s;

4.查询和姓名中包含字母u的员工再相同部门的员工号和姓名

SELECT employee_id,last_name
FROM employees
WHERE department_id in(

SELECT DISTINCT department_id
FROM employees
WHERE last_name LIKE '%u%'
);

5.查询在部门的location_id为1700的部门工作的员工的员工号

SELECT employee_id
FROM employees
where department_id in(
SELECT department_id
FROM departments
WHERE  location_id=1700
);

SELECT employee_id
FROM employees e
INNER JOIN departments d
ON e.department_id=d.department_id
WHERE location_id=1700;

6.查询管理者是King的员工姓名和工资

子连接：SELECT last_name
FROM employees
WHERE manager_id=ANY(
SELECT employee_id
from employees
WHERE last_name='K_ing'
);
自连接：SELECT e.last_name
FROM employees e
INNER JOIN employees m
on e.manager_id=m.employee_id
WHERE m.last_name='K_ing';

7.查询工资最高的员工姓名，要求first_name和last_name显示为一列，列名为姓，名

select concat(first_name,last_name)

from employees

where salary=()

select max(salary)

from employees

);

#### #进阶8 分页查询

/*

应用场景：当要显示的数据一页显示不全，需要分页提交sql请求

select 查询列表

from 表名

【join type  join 表二

where 筛选条件

groupby 分组字段

having 分组后筛选

orderby 排序字段】

limit offset,size;

offset要显示的条目的起始索引（从0开始）

size要显示的条目个数

limit  (page-1)*size,size;

*/

案例：有奖金的员工信息，并工资较高的前10名显示出来

select * from employee 

where commission_ptc is not null

orderby salary  DESC

limit 10;

1.查询平均工资最低部门的信息

方法1

select  avg(salary),department_id

from employees

group by department_id

having avg(salary)=(

select  min(avg_d.ag)

from

(

select avg(salary)  ag,department_id

from employees

group by department_id

)  avg_d

);

方法2：

select *

from department_id

wheredepartment=(

select department_id

from employees

group by department_id

order by avg(salary)

limit 1

);

3.查询平均工资最低部门的信息和该部门平均工资

select department_id，avg(salary)

from employees

group by department_id

order by avg(salary)

limit 1;

方法一：连接 select d*,ag

from ( select department_id，avg(salary) ag

from employees

group by department_id

order by avg(salary)

limit 1)  ag_d

inner join departments d

on  d.department_id=ag_d.department_id;

查询平均工资最高的job信息

select *

from jobs

where job_id=(

select job_id

from employee

group by job_id

order by avg(salary) DESC

limit 1;

)

查询平均工资超过公司平均工资的部门

SELECT avg(salary),department_id
FROM employees
where department_id is not null
group by department_id
HAVING avg(salary)>(
SELECT avg(salary)
FROM employees
);

查询公司中所有manager的详细信息

SELECT *
FROM employees
WHERE employee_id in 
(select DISTINCT manager_id
from employees
WHERE manager_id is not null);

7.各部门中最高工资中，找出最低工资

SELECT department_id,MIN(salary)
from employees
WHERE department_id=(
SELECT department_id
from employees
GROUP BY department_id
ORDER BY MAX(salary) 
limit 1)
GROUP BY department_id;

8.找出平均工资最高的部门的负责人信息

SELECT last_name,d.department_id,email,salary
FROM employees e
INNER JOIN departments d
on e.employee_id=d.manager_id
where d.department_id=(
SELECT department_id
from employees
GROUP BY department_id
ORDER BY avg(salary) DESC
LIMIT 1);

#### ###进阶9 联合查询

/*

union 联合 合并:将多条查询语句结果合并成一个结果

应用场景 要查询的结果来自多个表，且多个表没有直接关系，查询的值信息一致

特点：1.要求多条查询语句列一致2.要求多条查询语句查询每一列类型和顺序一致3.union默认去重，使用union all 可以包含重复项。

*/



#### #DML语言

/*

数据操作语言

插入:insert

修改:update

删除:delete

*/

##### 一.插入语句

/*

方式一：经典插入

语法

insert into 表名(列名,....)

values(值1,....)

*/

#1.insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)

values(13,'xxx','female','1999-3-1','1789927738939',null,2);

#2.不可以为null的值必须插入值，可以为null的列如何插入值?

1.直接赋予null

#1.insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)

values(13,'xxx','female','1999-3-1','1789927738939',null,2);

2.直接省略列与null

#1.insert into beauty(id,name,sex,borndate,phone,boyfriend_id)

values(13,'xxx','female','1999-3-1','1789927738939',2);

#列能够调换顺序；列数需要与值一致；可以省略列名，默认所有列，而且列的顺序和表中列顺序一致

方式二：

insert into 表名

set 列名=值，列名=值....

##两种方式特点

方式1：支持插入多行

方式2：