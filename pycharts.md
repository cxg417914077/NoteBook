[教程](http://pyecharts.org/#/zh-cn/prepare)

`pyecharts` 支持 `Python2.7+` 和 `Ptyhon3.5+`

`Bar`		   柱状图

`Line`		 折线图

```python
from pyecharts import Bar

bar = Bar("我的第一个图表", "这里是副标题") 	# 实例化Bar对象，添加标题和副标题
bar.add("服装", ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"], [5, 20, 36, 10, 75, 90], is_more_utils=True)
bar.use_theme('dark')
# bar.print_echarts_options() # 该行只为了打印配置项，方便调试时使用
bar.render('.html')    # 生成本地 HTML 文件
```

- `add()`
  主要方法，用于添加图表的数据和设置各种配置项.

  如果想要提供更多实用工具按钮，请在 `add()` 中设置 `is_more_utils` 为 True

  `is_stack=True`是否堆叠显示，需要将多个同时设置为`True`

- `print_echarts_options()`
  打印输出图表的所有配置项

- `render()`
  默认将会在根目录下生成一个 render.html 的文件，支持 path 参数，设置文件保存位置，如 render(r"e:\my_first_chart.html")，文件用浏览器打开。

- `use_theme()`

  更换主题色系

  图表类提供了若干了构建和渲染的方法，在使用的过程中，建议按照以下的顺序分别调用：

| 步骤 | 描述                                      | 代码示例               | 备注                                                         |
| ---- | ----------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| 1    | 实例一个具体类型图表的对象                | `chart = FooChart()`   |                                                              |
| 2    | 为图表添加通用的配置，如主题              | `chart.use_theme()`    |                                                              |
| 3    | 为图表添加特定的配置                      | `geo.add_coordinate()` |                                                              |
| 4    | 添加数据及配置项                          | `chart.add()`          | 参考 [数据解析与导入篇](http://pyecharts.org/#/zh-cn/data_import) |
| 5    | 生成本地文件（html/svg/jpeg/png/pdf/gif） | `chart.render()`       |                                                              |

从 v0.5.9 开始，以上涉及的方法均支持链式调用。例如：

```python
from pyecharts import Bar

CLOTHES = ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
clothes_v1 = [5, 20, 36, 10, 75, 90]
clothes_v2 = [10, 25, 8, 60, 20, 80]

(Bar("柱状图数据堆叠示例")
    .add("商家A", CLOTHES, clothes_v1, is_stack=True)
    .add("商家B", CLOTHES, clothes_v2, is_stack=True)
    .render())
```

从 v0.4.0+ 开始，pyecharts 重构了渲染的内部逻辑，改善效率。推荐使用以下方式显示多个图表。

```python
from pyecharts import Bar, Line
from pyecharts.engine import create_default_environment

bar = Bar("我的第一个图表", "这里是副标题")
bar.add("服装", ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"], [5, 20, 36, 10, 75, 90])

line = Line("我的第一个图表", "这里是副标题")
line.add("服装", ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"], [5, 20, 36, 10, 75, 90])

env = create_default_environment("html")
# 为渲染创建一个默认配置环境
# create_default_environment(filet_ype)
# file_type: 'html', 'svg', 'png', 'jpeg', 'gif' or 'pdf'

env.render_chart_to_file(bar, path='bar.html')
env.render_chart_to_file(line, path='line.html')
```

