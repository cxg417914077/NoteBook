## 初始化
所有Flask程序都必须创建一个程序实例。
```python
from flask import Flask
app = Flask(__name__)
```
函数视图的作用：生成请求的响应

## `jinja2`模板渲染引擎
html文件
```html
<h1>Hello, {{name}}</h1>
```
视图函数
```python
@app.route('user/<name1>')
def user(name1):
    return render_template('user.html', name=name1)
```
视图函数中传入的参数名必须与路径中的参数名一致
`render_template`函数中第一个参数为模板的文件名，随后的参数都是键值对，`name=name`，左边的`name`表示参数名，就是模板中使用的占位符，右边的`name`是当前作用域中的变量。`key`为模板文件中的变量名，`value`为传入函数内的变量名

## 模板语法
### 条件控制语句
```html
{ % if user % }
    Hello, {{ user }}!
{ % else % }
    Hello, Stranger!
{ % endif % }
```
### `for`循环语句
```html
<ul>
    { % for comment in comments % }
        <li>{{ comment }}</li>
    { % endfor % }
</ul>
```
### 宏
Jinja2 还支持宏。宏类似于`Python`代码中的函数。例如:
```html
{ % macro render_comment(comment) % }
    <li>{{ comment }}</li>
{ % endmacro % }
<ul>
    { % for comment in comments % }
        {{ render_comment(comment) }}
    { % endfor % }
</ul>
```
为了重复使用宏,我们可以将其保存在单独的文件中,然后在需要使用的模板中导入:
```html
{ % import 'macros.html' as macros % }
<ul>
    { % for comment in comments % }
        {{ macros.render_comment(comment) }}
    { % endfor % }
</ul>
```
需要在多处重复使用的模板代码片段可以写入单独的文件,再包含在所有模板中,以避免
重复:
```html
{ % include 'common.html' % }
```










`Arrow` Python时间插件
