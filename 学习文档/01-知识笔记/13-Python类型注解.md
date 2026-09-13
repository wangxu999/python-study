# Python 类型注解

## 一句话结论

类型注解用于说明变量、函数参数和返回值预期是什么类型。它能让代码更容易阅读，也能让 PyCharm 等工具提前提示可能的类型错误；但 Python 默认不会因为注解不匹配而阻止程序运行。

## 为什么使用类型注解

- 让别人和以后的自己更容易看懂数据应该是什么类型。
- 编写函数时，明确参数和返回值的预期类型。
- 编辑器可以根据注解给出补全和错误提示。
- 配合 `mypy`、Pyright 等工具，可以进行更严格的静态类型检查。

## 基本语法

变量的类型注解写在变量名后面，使用英文冒号 `:`。

```python
变量名: 类型 = 值
```

示例：

```python
name: str = "小王"
age: int = 18
height: float = 1.75
is_student: bool = True
```

也可以先声明类型，稍后再赋值：

```python
score: float
score = 95.5
```

## 函数的类型注解

函数参数的类型写在参数名后面；返回值类型写在右括号和英文冒号之间，使用 `->`。

```python
def 函数名(参数名: 类型) -> 返回值类型:
    return 返回值
```

示例：

```python
def calculate_total(price: float, count: int) -> float:
    return price * count


total: float = calculate_total(19.9, 3)
print(total)  # 59.7
```

函数没有需要返回的结果时，返回值类型写 `None`：

```python
def print_welcome(name: str) -> None:
    print(f"欢迎你，{name}")


print_welcome("小李")
```

## 常见类型注解

| 数据情况 | 类型注解示例 | 说明 |
| --- | --- | --- |
| 整数 | `int` | 如年龄、数量 |
| 小数 | `float` | 如价格、平均分 |
| 字符串 | `str` | 如姓名、地址 |
| 布尔值 | `bool` | 如是否登录 |
| 任意类型 | `object` | 可以接收任意对象，但具体操作前仍要确认类型 |
| 没有返回值 | `None` | 常用于只输出或修改数据的函数 |

## 容器的类型注解

Python 3.9 及以上版本可以直接用 `list`、`tuple`、`set`、`dict` 标注容器及其中元素的类型。

```python
names: list[str] = ["小王", "小李", "小张"]
scores: list[int] = [88, 95, 76]
tags: set[str] = {"Python", "基础"}
student: dict[str, int] = {"语文": 90, "数学": 96}
point: tuple[int, int] = (10, 20)
```

字典的写法中，前一个类型表示键，后一个类型表示值：

```python
student_scores: dict[str, float] = {
    "小王": 88.5,
    "小李": 95.0,
}
```

当元组中每个位置的类型不同，可以依次写出：

```python
user: tuple[str, int, bool] = ("小王", 18, True)
```

## 可能为空的值

有些变量平时是某种类型，但也可能没有值（`None`）。Python 3.10 及以上可用 `| None` 表示这种情况。

```python
nickname: str | None = None
nickname = "小王"
```

函数也可以返回某种类型或 `None`：

```python
def find_score(scores: dict[str, int], name: str) -> int | None:
    return scores.get(name)


result = find_score({"小王": 88, "小李": 95}, "小张")
print(result)  # None
```

使用前应判断结果是否为 `None`：

```python
if result is not None:
    print(result + 1)
else:
    print("没有找到该学生")
```

## 类型注解不会自动检查或转换

下面的代码可以运行，因为 Python 默认只把注解当作提示；但编辑器通常会提示类型不匹配。

```python
age: int = "十八"
print(age)  # 十八
```

类型注解也不会把字符串自动转换成整数：

```python
age: int = "18"  # 实际值仍然是字符串
print(type(age))  # <class 'str'>
```

从 `input()` 得到的数据始终是字符串，需要自己转换：

```python
age: int = int(input("请输入年龄："))
print(age + 1)
```

## 与类型检查的区别

| 功能 | `type()` / `isinstance()` | 类型注解 |
| --- | --- | --- |
| 主要作用 | 程序运行时判断实际类型 | 说明预期类型，辅助开发 |
| 检查时机 | 运行时 | 编写代码时或用工具检查时 |
| 能否阻止不匹配的赋值 | 可根据判断写出处理逻辑 | Python 默认不会阻止 |

需要根据真实数据作分支处理时，仍然要使用 `isinstance()` 等运行时检查。

```python
def show_value(value: object) -> None:
    if isinstance(value, int):
        print(value + 1)
    else:
        print("不是整数")
```

## 易错点

- [ ] 把类型注解当作数据类型转换。
  - 正确做法：`age: int = "18"` 不会转换字符串，需要写 `age = int("18")`。
- [ ] 认为注解不匹配时 Python 一定会报错。
  - 正确做法：Python 默认不会强制检查，注解主要服务于阅读和开发工具。
- [ ] 只标注函数参数，忘记标注返回值。
  - 正确做法：函数有明确结果时，同时写出 `-> 返回类型`。
- [ ] 只写 `list` 或 `dict`，没有说明容器中元素的类型。
  - 正确做法：尽量写成 `list[str]`、`dict[str, int]` 等更具体的形式。
- [ ] 对可能为 `None` 的结果直接做计算。
  - 正确做法：先用 `is not None` 判断，再继续使用。

## 自测问题

1. `name: str = "小王"` 中，`str` 表示什么？
2. 函数的返回值类型注解使用哪个符号？
3. 只打印内容、不返回结果的函数，返回值类型通常写什么？
4. `list[str]` 表示什么？
5. `dict[str, int]` 中的两个类型分别表示什么？
6. `str | None` 表示什么？
7. 为什么 `age: int = "18"` 后，`age` 的实际类型仍然是字符串？
8. 什么时候应使用 `isinstance()`，而不是只依赖类型注解？

## 关联内容

- 前置知识：常见数据类型、数据类型检查、数据容器、函数。
- 后续知识：类型别名、`typing` 模块、类与对象、静态类型检查工具。
- 练习：编写 `calculate_average(scores: list[float]) -> float` 函数，返回成绩列表的平均分；列表为空时自行设计处理方式。

## 复习状态

- 首次记录：2026-09-13
- 最近复习：
- 掌握程度：了解
