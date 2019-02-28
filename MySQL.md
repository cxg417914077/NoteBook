---
typora-copy-images-to: image
---

[TOC]

# MySQL必知必会笔记

> DBMS: 数据库管理系统
>
> 主键：一列（或一组列），其值能够唯一区分表中每个行。
>
> 外键：外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

## 使用MySQL

[将MySQL添加到环境变量](https://jingyan.baidu.com/article/3c48dd34782b68e10be35882.html)

**注意**：命令号执行`SQL`语句必须以分号结束，`SQL`语句不区分大小写

`mysql -hlocalhost -uusername -ppassword`

连接数据库：`localhost`:端口号，`username`:用户名， `password`:密码

`USE 数据库名;`

选择一个数据库

`SHOW DATABASES;`

返回可用数据库的额一个列表

`SHOW TABLES;`

返回当前选择的数据库内可用表的列表

`SHOW COLIMNS FROM 表名;`    =      `DESCRIBE 表名;`

对每个字段返回一行，包含字段名、数据类型、是否允许`NULL`、键信息、默认值以及其他信息

`SHOW STATUS;`

用于显示广泛的服务器状态信息

`SHOW CREATE DATABASE;`和`SHOW CREATE TABLE;`

分别用来显示创建特定数据库或表的`MySQL`语句

`SHOW GRANTS;`

用来显示授予用户（所有用户或特定用户）的安全权限

`SHOW ERRORS;`和`SHOW WARNINGS;`

用来显示服务器错误或警告消息

`HELP SHOW;`

显示允许的`SHOW`语句

## SQL语句顺序

### 书写顺序

```mysql
 1 SELECT DISTINCT
 2 FROM
 3 JOIN ON
 4 WHERE
 5 GROUP BY
 6 HAVING
 7 ORDER BY
 8 LIMIT
```

### 执行顺序

```mysql
 1 FROM
 2 ON
 3 JOIN
 4 WHERE
 5 GROUP BY
 6 HAVING
 7 SELECT
 8 DISTINCT
 9 ORDER BY
10 LIMIT
```

## 查询数据 （SELECT）

### 检索单个列

```mysql
SELECT 列名
FROM 表名;
```

### 检索多个列

```mysql
SELECT 列名,列名...
FROM 表名
```

列名之间加逗号，左后一个列名后面不加

### 检索所有列

```mysql
SELECT *
FROM 表名;
```

通配符（*）

检索不需要的列通常会降低检索和应用程序的性能

### 检索去重（DISTINCT）

```mysql
SELECT DISTINCT 列名
FROM 表名;
```

不能部分使用`DISTINCT`, `DISTINCT`后面两个列名时，同时作用两个字段，返回的是两个字段同时不相同的结果

### 分页（LIMIT）

`LIMIT M,N` 从m行开始返回n行

```mysql
SELECT 列名
FROM 表名
LIMIT 5;

SELECT 列名
FROM 表名
LIMIT 3,4;
```

带一个值的`LIMIT`总是从第一行开始，给出的数为返回的行数。即返回多少行

从第1行开始，返回5行

带两个值的`LIMIT `可以指定从行号为第一个值的位置开始。

从第3行开始，返回4行

### 完全限定的列名和表名

**完全限定的列名**

```mysql
SELECT products.prod_name
FROM products;
```

**完全限定的表名**

```mysql
SELECT products.prod_name
FROM crashcourse.products;
```

## 排序查询后的数据（ORDER BY）

### 排序

```MYSQL
SELECT 列名
FROM 表名
ORDER BY 列名1;
```

按照指定的列名1进行排序

```MYSQL
SELECT 列名
FROM 表名
ORDER BY 列名1,列名2;
```

先按照列名1排序，然后按照列名2排序

### 排序方向

默认按照升序`ASC`来进行排序

关键字`DESC`指定以降序排序

```mysql
SELECT 列名
FROM 表名
ORDER BY 列名1 DESC,列名2;
```

`DESC`关键字只应用到直接位于其前面的列名. 只对列名1进行降序，列名2按照默认的升序

如果想在多个列上进行降序排序，必须对每个列指定`DESC`关键字。

### `ORDER BY` 与`LIMIT`组合使用

使用ORDER BY和LIMIT 的组合，能够找出一个列中最高或最低的值。下面的例子演示如何找出最昂贵物品的值：

```mysql
SELECT prod_price
FROM products
ORDER BY prod_price DESC
LIMIT 1;
```

在给出 `ORDER BY`子句时，应该保证它位于`FROM`子句之后。如果使用`LIMIT` ，它必须位于`ORDER BY`之后。使用子句的次序不对将产生错误消息。



## 过滤数据（WHERE）

```mysql
SELECT 列名
FROM 表名
WHERE 过滤条件
```

​											**WHERE子句操作符**

| 操作符         | 说明               |
| -------------- | ------------------ |
| =              | 等于               |
| <>             | 不等于             |
| !=             | 不等于             |
| <              | 小于               |
| <=             | 小于等于           |
| >              | 大于               |
| >=             | 大于等于           |
| BETWEEN    AND | 在指定的两个值之间 |

### 检查单个值

```mysql
SELECT prod_name,prod_price
FROM products
WHERE prod_name = 'fuses';
```

返回爬prod_name值为fuses的那行

```mysql
SELECT prod_name,prod_price
FROM products
WHERE prod_price < 10;
```

返回价格小于10美元的所有产品

### 不匹配检查

```mysql
SELECT vend_id,prod_price
FROM products
WHERE vend_id <> 1003;
```

返回不是由制造商1003制造的所有产品

### 范围值检查

```mysql
SELECT prod_name,prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;
```

返回价格在5美元和10美元之间的所有商品，包括指定的开始值和结束值

### 空值检查

```mysql
SELECT prod_name
FROM products
WHERE prod_price IS NULL;
```

返回没有价格的所有产品

```mysql
SELECT prod_name
FROM products
WHERE prod_price IS NOT NULL;
```

返回有价格的所有产品

### 组合WHERE子句

> SQL （像多数语言一样）在处理OR操作符前，优先处理AND 操作符。可以用圆括号（）划分优先级

#### AND操作符

> AND 指示DBMS 只返回满足所有给定条件的行

```mysql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;
```

返回由制造商1003制造并且价格小于等于10美元的所有产品

#### OR操作符

> 检索匹配任一条件的行

```mysql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 1003 OR vend_id = 1002;
```

返回由制造商1003或1002制造的所有产品

#### IN操作符

> IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配

```mysql
SELECT prod_price, prod_name
FROM products
WHERE vend_id IN (1002, 1003)
ORDER BY prod_name;
```

返回制造商id在1002到1003之间的所有产品并按产品名排序

`IN`操作符的优点：

* 在使用长的合法选项清单时，IN操作符的语法更清楚且更直观。

* 在使用IN时，计算的次序更容易管理（因为使用的操作符更少）
* IN操作符一般比OR操作符清单执行更快。
* IN的最大优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE 子句。

#### NOT操作符

> NOT 否定跟在它之后的任何条件
>
> 支持使用NOT 对IN、BETWEEN 和EXISTS子句取反

```mysql
SELECT prod_price, prod_name
FROM products
WHERE vend_id NOT IN (1002, 1003)
ORDER BY prod_name;
```

返回1002到1003之外的供应商的产品

#### LIKE操作符（模糊查询）

> 使用通配符会降低检索效率

##### 百分号（%）通配符

> %表示任何字符出现任意次数，不能匹配NULL

```mysql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE 'jet%';
```

`jet%`表示以jet开始

`%jet`表示以jet结尾 

`%jet%`表示任意位置包含jet的

##### 下划线（_）通配符

> 总是匹配一个字符

#### 正则表达式（REGEXP）

> 可能会降低性能

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '正则表达式'
ORDER BY prod_name;
```

`.`  表示任意一个字符

`^ $` 定位符，表示开始和结尾

`|` OR 匹配

`[***]` 只匹配括起来的字符

`\\` 转义符

## 函数

`CONAT()`函数        

>  把多个串连接起来形成一个较长的串
>
> 需要一个或多个指定的串，各个串之间用逗号分隔
>
> 可以起别名

```mysql
SELECT CONCAT(vend_name, '(', vend_country, ')')
FROM products
ORDER BY vend_name;
```

`Trim()` 函数

> 删除数据左右两边多余的空格

`RTrim()` 函数

> 删除数据右侧多余的空格

`LTrim()` 函数

> 删除数据左侧多余的空格

```mysql
SELECT CONCAT(RTRIM(vend_name), '(', RTRIM(vend_country), ')')
FROM products
ORDER BY vend_name;
```

​											**常用的文本处理函数**

| 函数          | 说明                 |
| :------------ | -------------------- |
| `Left()`      | 返回字符串左边的字符 |
| `Right()`     | 返回字符串右边的字符 |
| `Lenght()`    | 返回字符串的长度     |
| `Locate()`    | 找出字符串的一个子串 |
| `SubString()` | 返回子串的字符       |
| `Upper()`     | 将字符串转换为大写   |
| `Lower()`     | 将字符串转换为小写   |
| `Soundex()`   | 返回字符串的SOUDEX值 |

> SOUNDEX 是一个将任何文本串转换为描述其语音表示的字母数字模式的算法

​											**常用日期和时间处理函数**

| 函数            | 说明                           |
| --------------- | ------------------------------ |
| `AddDate()`     | 增加一个日期（天、周等）       |
| `AddTime()`     | 增加一个时间（时、分等）       |
| `CurDate()`     | 返回当前日期                   |
| `CurTime()`     | 返回当前时间                   |
| `Date()`        | 返回日期时间的日期部分         |
| `DateDiff()`    | 计算两个日期之差               |
| `Date_Add()`    | 高度灵活的日期运算函数         |
| `Date_Format()` | 返回一个格式化的日期或时间串   |
| `Day()`         | 返回一个日期的天数部分         |
| `DayOfWeek()`   | 对于一个日期，返回对应的星期几 |
| `Hour()`        | 返回一个时间的小时部分         |
| `Minute()`      | 返回一个时间的分钟部分         |
| `Month()`       | 返回一个日期的月份部分         |
| `Now()`         | 返回当前日期和时间             |
| `Second()`      | 返回一个时间的秒部分           |
| `Time()`        | 返回一个日期时间的时间部分     |
| `Year()`        | 返回一个日期的年份部分         |

>  插入或更新表值还是用WHERE 子句进行过滤，日期必须为格式yyyy-mm-dd

​											**常用数值处理函数**

| 函    数 | 说    明           |
| -------- | ------------------ |
| `Cos()`  | 返回一个角度的余弦 |
| `Exp()`  | 返回一个数的指数值 |
| `Mod()`  | 返回除操作的余数   |
| `Pi()`   | 返回圆周率         |
| `Rand()` | 返回一个随机数     |
| `Sin()`  | 返回一个角度的正弦 |
| `Sqrt()` | 返回一个数的平方根 |
| `Tan()`  | 返回一个角度的正切 |

​												**SQL聚集函数**

| 函数      | 说明             |
| --------- | ---------------- |
| `AVG()`   | 返回某列的平均值 |
| `COUNT()` | 返回某列的行数   |
| `MAX()`   | 返回某列的最大值 |
| `MIN()`   | 返回某列的最小值 |
| `SUM()`   | 返回某列值之和   |

## 分组数据（GROUP BY）

`GROUP BY`的一些规定

* GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。 
* 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
* GROUP BY子句中列出的每个列都必须是检索列或有（但不能是聚集函数）。如果在SELECT中使用表达式GROUP BY子句中指定相同的表达式。不能使用别名。
* 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。 
* 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
* GROUP BY子句必须出现在WHERE 子句之后，ORDER BY子句之前。 

>  使用WITH ROLLUP 关键字，可以得到每个分组以及每个分组汇总级别（针对每个分组）的值

```mysql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP
```

### 过滤分组（HAVING）

`HAVING`支持所有`WHERE `操作符

`HAVING`和`WHERE` 的差别   这里有另一种理解方法，WHERE 在数据分组前进行过滤，HAVING在数据分组后进行过滤。这是一个重要的区别，WHERE 排除的行不包括在分组中。这可能会改变计算值，从而影响HAVING子句中基于这些值过滤掉的分组。

## 子查询（subquery）

> 嵌套在其他查询中的查询

```mysql
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id='TNT2'));
```



## 联结表（join）

### 笛卡尔积

由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

