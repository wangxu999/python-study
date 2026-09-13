# Python 字面量

## 一句话结论

字面量是直接写在 Python 代码中的固定值，例如 `10`、`3.14`、`True`、`"你好"` 和 `None`。Python 会根据书写形式识别它们的数据类型。

## 截图要点

| 分类 | 类型 | 含义 | 示例 |
| --- | --- | --- | --- |
| 数字类型 | `int` | 整数 | `10`、`18`、`0` |
| 数字类型 | `float` | 浮点数或小数 | `8.5`、`3.14`、`1.0` |
| 布尔类型 | `bool` | 表示真或假 | `True`、`False` |
| 字符串 | `str` | 表示文本 | `"人生苦短，我用Python"` |
| 空值 | `NoneType` | 表示没有值或暂未设置值 | `None` |
| 数据容器 | 多种类型 | 存储多项数据 | 列表、元组、集合、字典 |

## 基本示例

```python
age = 18                 # int
price = 3.14             # float
is_logged_in = True      # bool
message = "你好，Python"  # str
result = None            # NoneType

print(type(age))
print(type(price))
print(type(is_logged_in))
print(type(message))
print(type(result))
```

输出：

```text
<class 'int'>
<class 'float'>
<class 'bool'>
<class 'str'>
<class 'NoneType'>
```

## 各类字面量的书写规则

### 整数 `int`

整数没有小数点，例如 `10`、`0`。在代码里写 `-5` 可以得到负数 `-5`。

```python
score = 100
temperature = -5
```

### 浮点数 `float`

小数点会使数值成为浮点数；即使小数部分是零，`1.0` 仍然是 `float`，而 `1` 是 `int`。

```python
one = 1
one_point_zero = 1.0

print(type(one))
print(type(one_point_zero))
```

### 布尔值 `bool`

布尔值只有 `True` 和 `False` 两种。首字母必须大写，`true` 和 `false` 不是 Python 的布尔值。

```python
has_permission = True
is_empty = False
```

### 字符串 `str`

文本要写在单引号或双引号内。两种写法通常等价，但同一段代码中建议保持一致。

```python
city = "上海"
language = 'Python'
```

字符串中要包含双引号时，可以用单引号包围；反过来也一样。

```python
quote = '他说："你好"'
```

### 空值 `None`

`None` 表示“没有值”或“尚未得到结果”，它不是空字符串 `""`、数字 `0` 或布尔值 `False`。

```python
user_name = None  # 尚未获取用户名称
```

判断一个值是否为 `None` 时，使用 `is None`：

```python
if user_name is None:
    print("用户名尚未设置")
```

### 数据容器

数据容器用于保存多项数据。后续学习会分别讲解列表、元组、集合和字典；它们也有自己的字面量写法。

```python
names = ["小王", "小李"]                 # 列表 list
point = (10, 20)                          # 元组 tuple
tags = {"Python", "入门"}                 # 集合 set
student = {"name": "小王", "age": 18}   # 字典 dict
```

## 易错点

- [ ] 把 `True` 写成 `true`，或把 `False` 写成 `false`。
  - 正确做法：布尔值首字母必须大写。
- [ ] 忘记给字符串加引号。
  - 正确做法：文本必须用单引号或双引号包围。
- [ ] 认为 `1` 和 `1.0` 类型相同。
  - 正确做法：`1` 是 `int`，`1.0` 是 `float`。
- [ ] 用 `== None` 判断空值。
  - 正确做法：使用 `is None`。
- [ ] 把 `None` 与 `0`、`False` 或 `""` 混为一谈。
  - 正确做法：它们都是不同的值，也属于不同的概念或类型。

## 自测问题

1. `10`、`10.0`、`"10"` 分别是什么类型？
2. 为什么 `True` 不能写成 `true`？
3. `None` 和空字符串 `""` 有什么区别？
4. 写出一个包含姓名和年龄的字典字面量。
5. 使用 `type()` 检查下列值的类型：`False`、`"Python"`、`0`、`None`。

## 关联内容

- 前置知识：Python 注释、`print()` 函数。
- 后续知识：变量、`type()`、运算符、条件判断、数据容器。
- 练习：在 `D:\python-study\code\basic` 中新建文件，定义五种基本字面量并用 `type()` 输出类型。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
