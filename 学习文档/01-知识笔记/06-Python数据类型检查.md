# Python 数据类型检查

## 一句话结论

`type()` 用于查看数据的具体类型；`isinstance()` 用于检查数据是否属于指定类型，返回布尔值 `True` 或 `False`。

## type 语句

通过 `type()` 得到数据的类型。

语法：

```python
type(要查看类型的数据)
```

示例：

```python
age = 18
price = 8.5
name = "Python"

print(type(age))
print(type(price))
print(type(name))
```

输出：

```text
<class 'int'>
<class 'float'>
<class 'str'>
```

`type()` 返回的是类型对象，例如 `<class 'int'>`，不是 `True` 或 `False`。

## isinstance 语句

通过 `isinstance()` 检查数据是否属于指定类型，返回一个 `bool` 值。

语法：

```python
isinstance(数据, 类型)
```

示例：

```python
age = 18
price = 8.5

print(isinstance(age, int))
print(isinstance(age, str))
print(isinstance(price, float))
```

输出：

```text
True
False
True
```

## type 和 isinstance 的区别

| 对比项 | `type()` | `isinstance()` |
| --- | --- | --- |
| 主要用途 | 查看数据实际类型 | 判断数据是否属于某个类型 |
| 语法 | `type(data)` | `isinstance(data, data_type)` |
| 返回值 | 类型对象 | `True` 或 `False` |
| 示例结果 | `type(10)` 得到 `int` 类型 | `isinstance(10, int)` 得到 `True` |

## 在条件判断中使用 isinstance

当程序只允许某种类型的数据时，可以先用 `isinstance()` 检查。

```python
score = 95

if isinstance(score, int):
    print("分数是整数")
else:
    print("分数不是整数")
```

## 一次检查多种类型

第二个参数可以放入元组，表示数据只要属于其中一种类型即可。

```python
value = 3.14

print(isinstance(value, (int, float)))
```

输出：

```text
True
```

## 易错点

- [ ] 将 `type()` 和 `isinstance()` 的返回值混淆。
  - 正确做法：`type()` 返回类型对象；`isinstance()` 返回布尔值。
- [ ] 把类型写成字符串。
  - 错误示例：`isinstance(age, "int")`
  - 正确做法：`isinstance(age, int)`，`int` 不加引号。
- [ ] 忘记 `isinstance()` 的两个参数之间使用英文逗号。
  - 正确格式：`isinstance(数据, 类型)`。
- [ ] 直接比较 `type(data) == 类型` 来代替常规类型判断。
  - 正确做法：一般优先使用 `isinstance()`，它对后续学习的继承关系更友好。

## 自测问题

1. `type("Python")` 的结果表示什么？
2. `isinstance(8.5, float)` 返回什么？
3. 为什么 `isinstance(18, "int")` 是错误写法？
4. 如何检查变量是否是整数或浮点数？
5. `type()` 和 `isinstance()` 分别适合什么场景？

## 关联内容

- 前置知识：Python 常见数据类型、变量。
- 后续知识：条件判断、类型转换、面向对象。
- 练习：定义一个字符串、一个整数和一个小数，分别用 `type()` 与 `isinstance()` 检查它们。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
