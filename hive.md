# 概念

> spark hive kafuka

- Facebook开发
- 阿帕奇
- 基于Hadoop
- **数据仓库工具**
- **海量数据**
- 计算
- 类sql查询 ： **HQL**
- 本质是将HQL转化成MapReduce
- ![1582995391522](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1582995391522.png)
- 存数据在 HDFS
- 底层*默认*实现是MapReduce
- 程序运行在Yarn上
- HDFS为海量的数据提供了存储，则MapReduce为海量的数据提供了计算。

## 优点

- 大数据
- 学习快，SQL
- 延迟高，实效性差
- 自定义函数

## 缺点

- 不能迭代
- 数据挖掘不擅长
- 效率低MapReduce
- 调优困难

## 架构原理

![1583033216277](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1583033216277.png)

- 解析器  sql语法

- 编译器 将hql翻译成MR

- 优化器 

- 执行器 执行任务访问数据

  ![1583033437158](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1583033437158.png)

## 与数据库比较

### 查询语言

SQL与HQL

### 存储位置

云+本地 与 HDFS

### 数据更新

增删改查 与 查询

### 索引

有索引 与 暴力扫描整个数据

### 引擎

自己的执行引擎 与 MR

### 可扩展性

100台 与 4000台，建立在Hadoop之上

# 安装

> https://hive.apache.org/

