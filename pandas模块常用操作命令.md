# `pandas`常用操作命令



> `df`：任意的`Pandas DataFrame`对象
>
>
>
> `s`：任意的`Pandas Series`对象
>
>
> 同时我们需要做如下的引入：
>
>
> `import pandas as pd`
>
>
> `import numpy as np`



## 导入数据

* `pd.read_csv(filename， sep='\t', header=None, names=["a","b","c","d","name"],index_col="name")`：从`CSV`文件导入数据      默认分隔符为','   sep='\t' 设置默认分隔符为'\t'	header=None 读取没有标题的文件,表示没有标题

* `pd.read_table(filename)`：从限定分隔符的文本文件导入数据    默认分隔符为'\t'

- `pd.read_excel(filename)`：从Excel文件导入数据
- `pd.read_sql(query, connection_object)`：从`SQL`表/库导入数据
- `pd.read_json(json_string)`：从`JSON`格式的字符串导入数据
- `pd.read_html(url)`：解析`URL`、字符串或者`HTML`文件，抽取其中的tables表格
- `pd.read_clipboard()`：从你的粘贴板获取内容，并传给`read_table()`
- `pd.DataFrame(dict)`：从字典对象导入数据，`Key`是列名，`Value`是数据

```python
import pandas as pd
data = pd.read_csv(filename, sep='\t', header=None, names=['a', 'b', 'c', 'name'], index_col='name')
# read_csv()			默认分隔符为','
# read_table()			默认分隔符为'\t'
# sep='\t'				指定默认分隔符为'\t'   如果分隔符不规范可以用正则匹配
# header=None			读取没有标题的文件，设置该参数之后不会将第一行数据作为标题
# names=['a', 'name']	设置names参数，来设置文件的标题
# index_col='name'		设置index_col参数来设置列索引 将name列设置为索引。
#						如果不设置列索引，默认会使用从0开始的整数索引
#						可以设置多个索引列产生层次化索引
# skiprows([0,3,5])		设置跳过行，不读取[0,3,5]行
# pandas会默认将NA、-1.#IND、NULL等当作是缺失值，pandas默认使用NaN进行代替。
# na_values=['java', 'C++']	可以通过设置na_values，将指定的值替换成为NaN值	替换所有
# 只替换某一列，可以通过一个字典的形式来设置na_values参数，字典的键就是列索引，值就是你要替换的值。
# dic = {'name':['java', 'C++']}
# na_values=dic			只把name列的java和C++设置成NaN值

# path：表示文件系统位置、URL、文件型对象的字符串。
# sep或delimiter：用于对行中各字段进行拆分的字符序列或正则表达式。
# header：用作列名的行号。默认为0（第一行），如果文件没有标题行就将header参数设置为None。
# index_col：用作行索引的列编号或列名。可以是单个名称/数字或有多个名称/数字组成的列表（层次化索引）。
# names：用于结果的列名列表，结合header=None，可以通过names来设置标题行。
# skiprows：需要忽略的行数（从0开始），设置的行数将不会进行读取。
# na_values：设置需要将值替换成NA的值。
# comment：用于注释信息从行尾拆分出去的字符（一个或多个）。
# parse_dates：尝试将数据解析为日期，默认为False。如果为True，则尝试解析所有列。除此之外，参数可以指定需要解析的一组列号或列名。如果列表的元素为列表或元组，就会将多个列组合到一起再进行日期解析工作。
# keep_date_col：如果连接多列解析日期，则保持参与连接的列。默认为False。
# converters：由列号/列名跟函数之间的映射关系组成的字典。如,{"age:",f}会对列索引为age列的所有值应用函数f。
# dayfirst：当解析有歧义的日期时，将其看做国际格式（例如，7/6/2012   ---> June 7 , 2012）。默认为False。
# date_parser：用于解析日期的函数。
# nrows：需要读取的行数。
# iterator：返回一个TextParser以便逐块读取文件。
# chunksize：文件块的大小（用于迭代）。
# skip_footer：需要忽略的行数（从文件末尾开始计算）。
# verbose：打印各种解析器输出信息，如“非数值列中的缺失值的数量”等。
# encoding：用于unicode的文本编码格式。例如，"utf-8"或"gbk"等文本的编码格式。
# squeeze：如果数据经过解析之后只有一列的时候，返回Series。
# thousands：千分位分隔符，如","或"."。
```



## 导出数据

- `df.to_csv(filename)`：导出数据到`CSV`文件
- `df.to_excel(filename)`：导出数据到`Excel`文件
- `df.to_sql(table_name, connection_object)`：导出数据到`SQL`表
- `df.to_json(filename)`：以`Json`格式导出数据到文本文件



## 创建测试对象

- `pd.DataFrame(np.random.rand(20,5))`：创建20行5列的随机数组成的`DataFrame`对象
- `pd.Series(my_list)`：从可迭代对象`my_list`创建一个`Series`对象
- `df.index = pd.date_range('1900/1/30', periods=df.shape[0])`：增加一个日期索引



## 查看、检查数据

- `df.head(n)`：查看`DataFrame`对象的前n行
- `df.tail(n)`：查看`DataFrame`对象的最后n行
- `df.shape()`：查看行数和列数 # Windows加括号报错
- `df.info()`：查看索引、数据类型和内存信息
- `df.columns` 查看列
- `df.index `查看索引
- `df.describe()`查看数值型列的汇总统计会对数字进行统计显示总数最大最小差值
- `s.value_counts(dropna=False)`：查看`Series`对象的唯一值和计数
- `df.apply(pd.Series.value_counts)`：查看`DataFrame`对象中每一列的唯一值和计数



## 数据选取

- `df[col]`：根据列名，并以`Series`的形式返回列
- `df[[col1, col2]]`：以`DataFrame`形式返回多列
- `s.iloc[0]`：按位置选取数据 支持索引、切片
- `s.loc['index_one']`：按索引选取数据  没看懂这是什么鬼
- `df.iloc[0,:]`：返回第一行 冒号表示从头到尾，可以指定切片长度
- `df.iloc[0,0]`：返回第一列的第一个元素
- `df.iloc[:,0]`: 返回第一列数据  



## 数据清理

- `df.columns = ['a','b','c']`：重命名列名
- `pd.isnull().any()`：检查`DataFrame`对象中的空值，并返回一个`Boolean`数组
- `pd.notnull().any()`：检查`DataFrame`对象中的非空值，并返回一个`Boolean`数组
- `pd[pd.notnull() == True]` 过滤所有的空值
- `pd[pd.列名.notnull() == True]` 过滤本列中是空值得数据
- `df.dropna()`：删除所有包含空值的行
- `df.dropna(axis=1)`：删除所有包含空值的列
- `df.dropna(axis=1,thresh=n)`：删除所有小于n个非空值的行
- `df.fillna(x)`：用`x`替换`DataFrame`对象中所有的空值
- `s.astype(float)`：将`Series`中的数据类型更改为`float`类型
- `s.replace(1,'one')`：用`one`代替所有等于1的值 测试中将浮点数替换`int`  整列变成`int`类型
- `s.replace([1,3],['one','three'])`：用`one`代替1，用`three`代替3
- `df.rename(columns=lambda x: x + 1)`：批量更改列名
- `df.rename(columns={'old_name': 'new_ name'})`：选择性更改列名
- `df.set_index('column_one')`：更改索引列
- `df.rename(index=lambda x: x + 1)`：批量重命名索引





## 数据处理：`Filter`、`Sort`和`GroupBy`

- `df[df[col] > 0.5]`：选择col列的值大于0.5的行
- `df.sort_values(col1)`：按照列`col1`排序数据，默认升序排列
- `df.sort_values(col2, ascending=False)`：按照列`col1`降序排列数据
- `df.sort_values([col1,col2], ascending=[True,False])`：先按列`col1`升序排列，后按`col2`降序排列数据
- `df.groupby(col)`：返回一个按列`col`进行分组的`Groupby`对象 .  真的返回个队形地址
- `df.groupby([col1,col2])`：返回一个按多列进行分组的`Groupby`对象 .   
- `df.groupby(col1)[col2]`：返回按列`col1`进行分组后，列`col2`的均值 .   还是返回地址
- `df.pivot_table(index=col1, values=[col2,col3], aggfunc=max)`：创建一个按列`col1`进行分组，并计算`col2`和`col3`的最大值的数据透视表
  - `customer_data.pivot_table(index='refer', values='age', aggfunc=[max, min])` .  显示每个渠道的最大最小值
- `df.groupby(col1).agg(np.mean)`：返回按列`col1`分组的所有列的均值
  - 经常用于按渠道显示每个渠道的平均值，每个渠道的年龄平均值（最大最小不行整条数据）        
- `data.apply(np.mean)`：对`DataFrame`中的每一列应用函数`np.mean`
- `data.apply(np.max,axis=1)`：对`DataFrame`中的每一行应用函数`np.max`





## 数据处理：添加新列

* 根据当前处理结果将结果添加到新的列宗/增加一列
  - `frame['test'] = frame.apply(lamubda x: function(x.city, x.year), axis = 1)`
  - `function`是编写的函数



## 数据合并

* `df1.append(df2)`：将`df2`中的行添加到`df1`的尾部
* `df.concat([df1, df2],axis=1)`：将`df2`中的列添加到`df1`的尾部
* `df1.join(df2,on=col1,how='inner')`：对`df1`的列和`df2`的列执行`SQL`形式的`join`



## 数据统计

* `df.describe()`：查看数据值列的汇总统计
* `df.mean()`：返回所有列的均值
* `df.corr()`：返回列与列之间的相关系数
* `df.count()`：返回每一列中的非空值的个数
* `df.max()`：返回每一列的最大值
* `df.min()`：返回每一列的最小值
* `df.median()`：返回每一列的中位数
* `df.std()`：返回每一列的标准差  离散度
  - 数值越大表示数据越散