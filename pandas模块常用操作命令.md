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

* `pd.read_csv(filename)`：从`CSV`文件导入数据

* `pd.read_table(filename)`：从限定分隔符的文本文件导入数据

- `pd.read_excel(filename)`：从Excel文件导入数据
- `pd.read_sql(query, connection_object)`：从`SQL`表/库导入数据
- `pd.read_json(json_string)`：从`JSON`格式的字符串导入数据
- `pd.read_html(url)`：解析`URL`、字符串或者`HTML`文件，抽取其中的tables表格
- `pd.read_clipboard()`：从你的粘贴板获取内容，并传给`read_table()`
- `pd.DataFrame(dict)`：从字典对象导入数据，`Key`是列名，`Value`是数据



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