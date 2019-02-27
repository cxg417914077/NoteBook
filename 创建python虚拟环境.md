# python虚拟环境

虚拟环境使用第三方实用工具`virtualenv`创建

执行`virtualenv --version`检查系统是否安装了`virtualenv`

安装`virtualenv`,执行命令`sudo apt-get install python-virtualenv`

创建一个名为`venv`的虚拟环境`virtualenv venv `

进入虚拟环境`source venv/bin/activate`

退出虚拟环境[deactivate]()



## 导出和导入Python环境安装包

### 导出Python环境安装包

`pip freeze > packages.txt`

这将会创建一个 `packages.txt`文件，其中包含了当前环境中所有包及各自的版本的简单列表（即pip list 所列出的包列表）

### 安装导入Python环境包

`pip install -r packages.txt`