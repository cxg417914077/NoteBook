# `flask`数据库操作

## 定义模型

## 配置数据库、定义模型
```python
# 配置数据库
from flask_sqlalchemy import SQLAlchemy
import os

basedir  os.path.abspath(os.path.dirname(__file__))

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] =\
    'sqlite:///' + os.path.join(basedir, 'data.sqlite')
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True

db = SQLAlchemy(app)


# 定义模型类
class Role(db.Model):
    __tablename__ = 'roles'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)

    def __repr__(self):
        return '<Role %s>' % self.name


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, index=True)
   
    def __repr__(self):
        return '<User % r>' % self.username
```

## 创建表
`db.create_all()`

## 删除表
`db.drop_all()`



## 插入表

## SQLAlchemy列类型
|  类型名  |  Python类型   |  说明  |
|:-|:-|:-|
Integer| int| 普通整数,一般是 32 位
SmallInteger| int| 取值范围小的整数,一般是 16 位
BigInteger int 或 long 不限制精度的整数
Float float 浮点数
Numeric decimal.Decimal 定点数
String str 变长字符串
Text str 变长字符串,对较长或不限长度的字符串做了优化
Unicode unicode 变长 Unicode 字符串
UnicodeText unicode 变长 Unicode 字符串,对较长或不限长度的字符串做了优化
Boolean bool 布尔值
Date datetime.date 日期
Time datetime.time 时间
DateTime datetime.datetime 日期和时间
Interval datetime.timedelta 时间间隔
Enum str 一组字符串
PickleType 任何 Python 对象 自动使用 Pickle 序列化
LargeBinary str 二进制文件
