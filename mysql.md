| base                          | select              | alter                                                        |      |      |      |
| ----------------------------- | ------------------- | ------------------------------------------------------------ | ---- | ---- | ---- |
| [修改执行符号](#修改执行语句) | [limit](#limit)     | [modify](#列属性)                                            |      |      |      |
| [登录](#login)                | [distinct](#唯一指) | [rename](#修改表名：alter table old_table rename to new_table;) |      |      |      |
|                               |                     | [add](#添加列：alter table 表名 add column 列名 varchar(30);) |      |      |      |
|                               |                     | [drop](#删除列：alter table 表名 drop column 列名;)          |      |      |      |



###### login

```cmd
mysql -u tsxx736 -h rm-uf641076h06m5vvtayo.mysql.rds.aliyuncs.com -p
```

```python
account = ("rm-uf641076h06m5vvtayo.mysql.rds.aliyuncs.com", "tsxx736", '******', 'interest')
```

###### **返回字典**

```python
cursor(cursor = pymysql.cursors.DictCursor)
```

##### 日期加减

http://www.360doc.com/content/19/0304/13/13328254_819122049.shtml

|        |                     |
| ------ | ------------------- |
| 日期减 | datediff( d1 , d2 ) |
|        |                     |
|        |                     |

###### 数学

|      |      |
| ---- | ---- |
| //   | div  |
| %    | mod  |
|      |      |

![img](https://images2015.cnblogs.com/blog/822428/201512/822428-20151231094010557-1124781943.png)

###### 修改执行语句

```mysql
DELIMITER $$
```

#### 查

###### if 判断

```mysql
select if(c_custname='薇薇',0,1) from erp;

```

###### 查询结果加序号

```python
select   (@i:=@i+1)   as   i,t.n_sale   from   erp t,(select   @i:=0)   as   it ;
```

​	

###### limit

```mysql
select * from tableName limit i,n
```

###### 唯一指

```mysql
select distinct n_sale from erp;
```

#### 改

##### 修改列

###### 列属性

```mysql
ALTER TABLE tb_article MODIFY COLUMN NAME CHAR(50);  
```

##### 表

##### drop

drop table  if exists user  ;

###### 复制表

```mysql
create table badsun like sunning1;

insert into badsun select * from sunning1 where qualityStar=1;
```

###### 修改表名：alter table old_table rename to new_table;

###### 添加列：alter table 表名 add column 列名 varchar(30);

###### 删除列：alter table 表名 drop column 列名;

###### 笛卡尔积：select from 2表

```
SELECT *
FROM Employee AS a, Employee AS b
;
```

#### 字段截取

```
1、left(str,index) 从左边第index开始截取

2、right(str,index)从右边第index开始截取

3、substring(str,index)当index>0从左边开始截取直到结束  当index<0从右边开始截取直到结束  当index=0返回空

4、substring(str,index,len) 截取str,从index开始，截取len长度

5、substring_index(str,delim,count)，str是要截取的字符串，delim是截取的字段 count是从哪里开始截取(为0则是左边第0个开始，1位左边开始第一个选取左边的，-1从右边第一个开始选取右边的

6、subdate(date,day)截取时间，时间减去后面的day

7、subtime(expr1,expr2)  时分秒expr1-expr2
```

筛选后join

```mysql
	select a.n_sale,a.c_custname from erp a inner join ( select c_custname,count(c_custname) from real_show group by c_custname having count(c_custname)>3
	 ) b on a.c_custname = b.c_custname group by a.c_custname;
```

##### where in(...,...,....)

```mysql
select emp_name,salary from employee where salary in(3000,2500,4000,9000);
```

##### like

```mysql
    5.模糊查询 like '%' 通配符
        like '%a'  匹配以a结尾的任意长度的字符串
        like 'a%'  匹配以a开头的任意长度的字符串
        like '%a%' 匹配字符串中含有a字符的字符串
        like '_a'  一共是2个长度,以a结尾,前面那个字符是什么无所谓
        like 'a__' 一共是3个长度,以a开头,后面是什么字符无所谓
```

##### 复合排序

```
select * from employee order by age asc , hire_date desc
```

##### concat

```mysql
# (7) concat sql内置函数 concat(参数1,参数2,参数3) 把所有参数拼接在一起
# as 用来起别名
select emp_name,concat("姓名:",emp_name,"薪水:",salary) as ss  from employee;
# concat_ws("符号",参数1,参数2,参数3) 第一个参数是分隔符,后面写上要拼接的参数
select emp_name,concat_ws(" : ",emp_name,salary) as aa from employee;
# 在sql中可以做四则运算(+ - * /)
select emp_name,concat_ws(" : ",emp_name,salary*12) as aa from employee;
```

##### 子查询

>    标量子查询（结果集只有一行一列）
>    列子查询（结果集只有一列多行）
>    行子查询（结果集有一行多列）
>    表子查询（结果集一般为多行多列）

本质上就是where+条件 可以加and if

######  标量子查询

```mysql
SELECT *
FROM employees
WHERE salary>(
 
    SELECT salary
    FROM employees
    WHERE last_name = 'Abel'
 
);

```

###### 列子查询

酒店中金额比任意电话卡小的

```mysql
select n_SALE,c_agentname from erp where n_sale<any(
select distinct n_SALE from erp where c_discode like '%卡%')
and c_countyname = '.0';
```

酒店中金额比所有电话卡小的

```mysql
select * from erp where n_sale<all(
select distinct n_SALE from erp where c_discode like '%卡%')
and c_countyname = '.0';
```

或

```mysql
select * from erp where n_sale<min(
select distinct n_SALE from erp where c_discode like '%卡%')
and c_countyname = '.0';
```

###### 行子查询

```mysql
SELECT *
FROM employees
WHERE employee_id=(
    SELECT MIN(employee_id)
    FROM employees
 
 
)AND salary=(
    SELECT MAX(salary)
    FROM employees
 
);
 

```

或

```mysql
SELECT * 
FROM employees
WHERE (employee_id,salary)=(
    SELECT MIN(employee_id),MAX(salary)
    FROM employees
);

```

###### where

###### select 后面

```mysql
select c_city,(select count(c_city) from erp e1 where e1.c_city=e2.c_city) from erp e2

```

![image-20200303124734620](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200303124734620.png)

###### from 后面 （必须命名）

```mysql
SELECT  ag_dep.*,g.`grade_level`
FROM (
    SELECT AVG(salary) ag,department_id
    FROM employees
    GROUP BY department_id
) ag_dep
INNER JOIN job_grades g
ON ag_dep.ag BETWEEN lowest_sal AND highest_sal;

```

#### 窗口函数

```mysql
# topN问题 sql模板
select *
from (
   select *, 
          row_number() over (partition by 要分组的列名
                       order by 要排序的列名 desc) as 排名
   from 表名) as a
where 排名 <= N;


```

