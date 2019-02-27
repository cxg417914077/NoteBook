[TOC]

select

from

on

join

group by

having

order by

distinct

limit



### 一对多建表

多的一方创建外键，指向一的主键

### 加外键的缺点：

数据必须一致

内连接，笛卡尔集

`inner join`

外连接：

左外连接：

`left join ... on`

右外连接：

`right join ... on`

A表独有



B表独有

全连接

不支持全连接，但是可以实现这种效果 `union`

左外连接   `UNION` 右外连接      （自动去除重复数据）

A独有+B独有

A表独有 `UNION` B表独有





## 索引

> `MySQL`高效获取数据的数据结构
>
> 本质：数据结构     B树

### 好处

1. 快、准
2. 缩小查找范围

二叉树（一个节点最多只有两个子节点）

左子树、右子树

根节点（没有父节点的节点）、子节点、叶子节点（没有子节点的节点）

#### B树



### 分类

**单值索引**

​	一个索引只包含单个列，一个表可以有多个单列索引

**唯一索引**

​	索引列的值必须唯一，但允许有控制

**复合索引**

​	一个索引包含多个列

### 基本语法

**创建**

`CREATE [UNIQUE] INDEX indexname ON mytable(col01,col02,col03....);`   常用

`ALTER TABLE 表名 ADD [UNIQUE] INDEX [indexname] ON [columnname(length)]`

**删除**

`DROP INDEX [indexname] ON mytable;`

**查看**

`SHOW INDEX FROM table_name`

### 索引创建注意事项

1. 一般来讲，不要再一张表里面创建过多单值索引
   1. 如果一个表单值索引过多，会造成查询效率降低	5-8

## EXPLAIN

`explain` + `SQL`语句

### 执行结果解析

**`id`字段**

