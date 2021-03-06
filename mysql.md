



# 优化

## linux配置

## MYSQL分层、存储引擎

### 分层知识点

1. 连接层
2. 服务层
   1. 提供这次查询的业务接口
   2. 提供SQL优化器 （Mysql Query Optimizer）
3. 引擎层
   1. *InnoDB*   **事务** **并发**  锁：行锁
   2. MyISAM **查询效率**   锁：表锁
4. 存储层

### 引擎设置

当前支持引擎

```mysql
show engines;
```

当前使用引擎

```mysql
 show variables like '%storage_engine%'
```

​	指定表 引擎

```mysql
create table sample(
id int(4) auto_increment,
    name varchar(5),
    dept varchar(5),
    primary key (id)
)ENGINE=MyISAM  /* 设置引擎 */
AUTO_INCREMENT=1 /*自增步长*/
DEFAULT CHARSET =utf8
;
```

## 常见问题

- 链接查询
- 索引失效
- 服务参数设置不佳

## sql语句与引擎实际解析

```mysql
/*语法*/
select..from..join..on..where..group by..having..order by ..

/* 解析 */
from .. on ..join..where..group by ...having ..selcet  ..order by 

```

## 索引

### 概念

- primary key 也是一种唯一索引

- 相当于目录

- index

- 官方：索引是一种帮助sql高效获取数据的数据结构 

  - 索引是树 B树（默认）

  - ​                50

  - ​              /      \

  - ​         28         76                   *这不是B树*

  - ​        /   \         /   \

  -    13      32   **54**    99

  -    /   \

  - 1      14

     *打比方*   实例： 减低IO使用率

    | id |

    | 1|

    | 2|

    ...

    | **54**|

    select * from .. where id = 54 :sweat_smile: 会历遍id字段 需要54步

    如果是在树中查找 50:arrow_right:75 :arrow_right: **54**  只需要3步 :laughing:

  - 索引指向行所在的存储空间 like 0x3a  ,  0xf4.....

### 索引列注意事项

- 不要选择频繁更行的字段
- 不要用很少使用的字段
- :exclamation: 索引会降低 增删改 的效率

### 优势

- 提升IO使用率
- 降低CPU使用率

### 分类

- 单值索引  （单列 一个表可以有多个）
- 唯一索引 （不能重复 一般用ID）
- 符合索引 （多个列构成的索引）

### 语法

- add
  - 方式一	

    ```mysql
    create /* + unique 为唯一索引*/ index  index_name on erp(id)  /*单值索引*/
    
    ```


```mysql
    create index index_name on erp(c_city,id)    /*复合索引*/
    
```

  - 	方式二

    ```mysql
    alter table erp add /*unique*/index index_name(dept,id)
    ```

- 删除

  ```mysql
  drop index index_name on erp;
  ```

- 查看

  ```mysql
  show index from erp;
  ```

## 优化计划 ID tpye..

- ```mysql
  explain sql语句
  ```

1. ID

   1. ID大小
      1. 相同，从上往下顺序执行
      2. 不同 从大往小	
   2. 小表驱动大表
   3. 子查询 先内层再外层

2. select_type 查询类型

   1. :notebook_with_decorative_cover: PRIMARY 主查询
   2. :business_suit_levitating: SUBQUERY

3. type 索引类型

   1. system >const>eq ref >**ref** >**range**>**index**>all

      1. system,const  实际不可能达到

      2. eq ref  常见于 **唯一索引** 和**主键索引**

         1. ```mysql
            alter table erp add constraint xxxx primary key(id)
            ```

         2. 

            ```mysql
            alter table erp add constraint xxxx unique index(id)
            ```

      3. ref 

         - 非唯一索引
         - 返回多行

      4. range

         - where 后 索引列
           - between
           - in   (有时候会索引失效)
           - \>
           - \<

      5. index  

         - 查询索引列  

         - ```mysql
           select ID from erp
           ```

      6. all

         - 查询非索引所有数据
         - select N_sale from erp

4. possible key

5. key 实际用到的索引

   

------



# 基础

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

| pow（a,b） | a**b |
| ---------- | ---- |
| //         | div  |
| %          | mod  |
|            |      |

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

##### 结果加序号

```mysql
简单实例：select  (@i:=@i+1)  i,user_id from  erp ,(select   @i:=0)   as   it ORDER BY t.user_id desc;
```

##### 联合group

##### 唯一指

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

### 高级操作

#### leetcode

https://leetcode-cn.com/problems/queries-quality-and-percentage/comments/ --两表筛选后join会丢失数据

https://leetcode-cn.com/problems/game-play-analysis-iii/comments/ -- @i:=xxx 判断

#### ot

##### 字段截取

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

#### 子查询

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

#### case

```mysql
 select *,case when brand='宝马' then '高级品牌' 
 when brand='宝马' then '高级品牌' 
 when brand='宝马' then '高级品牌' 
 when brand='宝马' then '高级品牌' 
 
 
 
 else '我觉得普通' end com from bp;

```

or

```
 select *,case brand when '宝马' then '高级品牌' 
 when '宝马' then '高级品牌'
 when '宝马' then '高级品牌'
 when '宝马' then '高级品牌'
 
 
 
 else '我觉得普通' end com from bp;
```

###### case有elif的作用

```mysql
select *,
case 
	when p>3000 then 'super'
	when p>1000 then 'good'
	else 'normal'
end mark


 from bp

```



#### if

```mysql
select if((....or....and....),'yes','no') from .....
```





#### select 单字

```mysql
select 
(
select sum(花费分)/100 as card_car from run_car where 宝贝名称 like '%电话卡%' 
),
(
select sum(erp.n_sale) as all_card_sell from erp where c_discode like '%电话卡%'
)
```

####  (select @i:=100)

```mysql
 select (@i:=@i+5),bp.* from  (select @i:=100)a,bp
```

+ ###### if

+ 

  ```mysql
  select if(c_countyname='日本',(@i:=@i+1),0),a.* from
  
  (select cast(d_date as date) as dd,c_countyname,sum(n_sale) from erp where cast(d_date as date)<'2020-1-6' group by dd,c_countyname order by dd)a,
  (select @i:=1)b
  ```

  

+----------------------------------------+------------+--------------+-------------+
| if(c_countyname='日本',(@i:=@i+1),0)   | dd         | c_countyname | sum(n_sale) |
+----------------------------------------+------------+--------------+-------------+
|                                      0 | 2020-01-01 | .0           |       13086 |
|                                      0 | 2020-01-01 | 台湾         |          68 |
|                                      0 | 2020-01-01 | 塞班         |         190 |
|                                      0 | 2020-01-01 | 新加坡       |         397 |
|                                      2 | 2020-01-01 | 日本         |        1814 |
|                                      0 | 2020-01-01 | 欧洲18国     |        1584 |
|                                      0 | 2020-01-01 | 泰国         |        2410 |
|                                      0 | 2020-01-01 | 美国         |         520 |
|                                      0 | 2020-01-01 | 韩国         |        1145 |
|                                      0 | 2020-01-02 | .0           |       10330 |
|                                      0 | 2020-01-02 | 东南亚       |         233 |
|                                      0 | 2020-01-02 | 全球         |         162 |
|                                      0 | 2020-01-02 | 新加坡       |        1351 |
|                                      3 | 2020-01-02 | 日本         |        2674 |
|                                      0 | 2020-01-02 | 欧洲18国     |        2515 |
|                                      0 | 2020-01-02 | 泰国         |        3695 |
|                                      0 | 2020-01-02 | 美国         |         970 |
|                                      0 | 2020-01-02 | 阿联酋       |         636 |

+ case

  ```mysql
  
  select a.*,
  case
  	when @i = id then @n:=@n+1
  	when @i := id then @n:=1
  end mytry
  from
  
  
  (select * from game order by id
  )a,
  (select @i:=0,@n:=0)b
  
  ```

  +------+------+-------+
  | id   | num  | mytry |
  +------+------+-------+
  |    1 |    1 |     1 |
  |    2 |    0 |     1 |
  |    3 |    1 |     1 |
  |    3 |    1 |     2 |
  |    3 |    1 |     3 |
  |    4 |    1 |     1 |
  +------+------+-------+

#### union 、 union all

```
select * from erp union all selet * from allerp
```

###### 用户变量if

```mysql
select d.Name as Department,e.Name as Employee,e.Salary
from Employee as e 
inner join Department as d 
on e.DepartmentId = d.Id 
inner join (
    select DepartmentId,Salary,@n := if(DepartmentId = @pre,@n+1,1),@pre := DepartmentId
    from
    (
        select distinct DepartmentId,Salary
        from Employee 
        order by DepartmentId,Salary desc
    ) as t1,(select @n := 1,@pre := null) as t2
    where @n < 3 or DepartmentId <> @pre
) as t 
on e.DepartmentId = t.DepartmentId and e.Salary = t.Salary
order by Department,Salary desc;


```

###### 自定义列

```mysql
(select 'desktop' as platform union
 select 'mobile' as platform union
 select 'both' as platform
)

 
```

按指定顺序排序

```mysql
order by field(platform,'desktop','mobile','both')
```

