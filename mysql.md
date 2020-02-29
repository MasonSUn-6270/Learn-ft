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



###### 修改执行语句

```mysql
DELIMITER $$
```

#### 查

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