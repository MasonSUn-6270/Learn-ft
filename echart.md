|              [bar](#bar)              |                           | annotate |      |      |
| :-----------------------------------: | ------------------------- | -------- | ---- | ---- |
|         [基本示例](#基本示例)         | [动画](#动画配置基本示例) |          |      |      |
|       [从dict配置](#从dict配置)       | [背景图](#背景图)         |          |      |      |
|          [Toolbox](#Toolbox)          |                           |          |      |      |
|   [单系列柱间距离](#单系列柱间距离)   |                           |          |      |      |
| [不同系列柱间距离](#不同系列柱间距离) |                           |          |      |      |
|       [ticklabels](#ticklabels)       |                           |          |      |      |
|            [label](#label)            |                           |          |      |      |
|                [H](#H)                |                           |          |      |      |
|       [stack-all-part](#stack)        |                           |          |      |      |
|                                       |                           |          |      |      |
|                                       |                           |          |      |      |
|                                       |                           |          |      |      |
|                                       |                           |          |      |      |
|                                       |                           |          |      |      |

###### bar

```python
from pyecharts import options as opts
from pyecharts.charts import Bar, Page
from pyecharts.commons.utils import JsCode
from pyecharts.faker import Collector, Faker
from pyecharts.globals import ThemeType

C = Collector()
```

###### 基本示例

```python
@C.funcs
def bar_base() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-基本示例", subtitle="我是副标题"))
    )
    return c
```

![image-20200226140238431](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226140238431.png)

###### 动画配置基本示例

```python
@C.funcs
def bar_base_with_animation() -> Bar:
    c = (
        Bar(
            init_opts=opts.InitOpts(
                animation_opts=opts.AnimationOpts(
                    animation_delay=1000, animation_easing="elasticOut"
                )
            )
        )
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Bar-动画配置基本示例", subtitle="我是副标题")
        )
    )
    return c
```

###### 背景图

```python
@C.funcs
def bar_base_with_custom_background_image() -> Bar:
    c = (
        Bar(
            init_opts=opts.InitOpts(
                bg_color={
                    "type": "pattern",
                    "image": JsCode("img"),
                    "repeat": "no-repeat",
                }
            )
        )
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(
                title="Bar-背景图基本示例",
                subtitle="我是副标题",
                title_textstyle_opts=opts.TextStyleOpts(color="white"),
            )
        )
    )
    c.add_js_funcs(
        """
        var img = new Image(); img.src = 'https://s2.ax1x.com/2019/07/08/ZsS0fK.jpg';
        """
    )
    return c
```

![image-20200226140827282](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226140827282.png)

###### 从dict配置

```python
@C.funcs
def bar_base_dict_config() -> Bar:
    c = (
        Bar({"theme": ThemeType.MACARONS})
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts={"text": "Bar-通过 dict 进行配置", "subtext": "我也是通过 dict 进行配置的"}
        )
    )
    return c
```

###### Toolbox

```python
@C.funcs
def bar_toolbox() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Bar-显示 ToolBox"),
            toolbox_opts=opts.ToolboxOpts(),
            legend_opts=opts.LegendOpts(is_show=False),
        )
    )
    return c
```

![image-20200226141632252](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226141632252.png)

###### 单系列柱间距离



```
@C.funcs
def bar_same_series_gap() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values(), category_gap="80%")
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-单系列柱间距离"))
    )
    return c
```

###### 不同系列柱间距离

```
@C.funcs
def bar_different_series_gap() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values(), gap="0%")
        .add_yaxis("商家B", Faker.values(), gap="0%")
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-不同系列柱间距离"))
    )
    return c
```

![image-20200226142343824](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226142343824.png)

###### ticklabels

```python
@C.funcs
def bar_yaxis_formatter() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Bar-Y 轴 formatter"),
            yaxis_opts=opts.AxisOpts(
                axislabel_opts=opts.LabelOpts(formatter="{value} /月")
            ),
        )
    )
    return c
```

![image-20200226142431189](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226142431189.png)

###### label

```
@C.funcs
def bar_xyaxis_name() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Bar-XY 轴名称"),
            yaxis_opts=opts.AxisOpts(name="我是 Y 轴"),
            xaxis_opts=opts.AxisOpts(name="我是 X 轴"),
        )
    )
    return c
```

###### H

```
@C.funcs
def bar_reversal_axis() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values())
        .add_yaxis("商家B", Faker.values())
        .reversal_axis()
        .set_series_opts(label_opts=opts.LabelOpts(position="right"))
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-翻转 XY 轴"))
    )
    return c
```

![image-20200226142805655](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226142805655.png)

###### stack

```
@C.funcs
def bar_stack0() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values(), stack="stack1")
        .add_yaxis("商家B", Faker.values(), stack="stack1")
        .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-堆叠数据（全部）"))
    )
    return c


@C.funcs
def bar_stack1() -> Bar:
    c = (
        Bar()
        .add_xaxis(Faker.choose())
        .add_yaxis("商家A", Faker.values(), stack="stack1")
        .add_yaxis("商家B", Faker.values(), stack="stack1")
        .add_yaxis("商家C", Faker.values())
        .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-堆叠数据（部分）"))
    )
    return c
```

![image-20200226143354818](C:\Users\sunjiahao\AppData\Roaming\Typora\typora-user-images\image-20200226143354818.png)

