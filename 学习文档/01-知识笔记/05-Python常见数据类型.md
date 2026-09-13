# Python 常见数据类型

## 一句话结论

数据类型决定一个值可以表示什么，以及可以进行哪些操作。Python 中常见的基础数据类型有 `int`、`float`、`str`、`bool` 和 `NoneType`。

## 常见基础数据类型

| 标准名称 | 中文描述 | 用途与书写示例 |
| --- | --- | --- |
| `int` | 整数 | 数字类型，存放整数，例如 `10`、`-5`、`0` |
| `float` | 浮点数 | 数字类型，存放小数，例如 `8.5`、`3.14`、`1.0`、`-3.5` |
| `str` | 字符串 | 用引号引起来的文本，例如 `"Python"`、`'你好'` |
| `bool` | 布尔 | 描述真和假，只有 `True` 和 `False` |
| `NoneType` | 空值 | 表示空或无值，仅包含一个值 `None` |

## 示例代码

```python
age = 18                    # int，整数
temperature = -3.5          # float，浮点数
language = "Python"         # str，字符串
is_finished = False          # bool，布尔值
result = None                # NoneType，空值

print(type(age))
print(type(temperature))
print(type(language))
print(type(is_finished))
print(type(result))
```

输出：

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
<class 'NoneType'>
```

## 重点说明

### 整数 `int`

整数不带小数点，例如 `10`、`-5`。`10-5` 不表示一个整数的写法，而是“10 减 5”的计算表达式，结果才是整数 `5`。

```python
number = 10 - 5
print(number)       # 5
print(type(number)) # int
```

### 浮点数 `float`

只要带有小数点，通常就是浮点数。`1` 是整数，而 `1.0` 是浮点数。

### 字符串 `str`

用单引号或双引号包围的内容都是字符串，包括看起来像数字的文本。

```python
number = 10      # int
text_number = "10"  # str
```

### 布尔值 `bool`

`True` 表示真，`False` 表示假。它们的首字母必须大写。

### 空值 `NoneType`

`None` 表示没有值或暂时没有结果。判断变量是否为 `None` 时，使用 `is None`。

```python
result = None

if result is None:
    print("当前没有结果")
```

## 易错点

- [ ] 认为 `10` 和 `"10"` 是同一类型。
  - 正确做法：`10` 是 `int`，`"10"` 是 `str`。
- [ ] 认为 `1` 和 `1.0` 是同一类型。
  - 正确做法：`1` 是 `int`，`1.0` 是 `float`。
- [ ] 将 `True`、`False` 写成小写。
  - 正确做法：首字母必须大写。
- [ ] 将 `None` 和 `0`、`False`、空字符串 `""` 混淆。
  - 正确做法：它们是不同的值，含义也不同。

## 自测问题

1. `-5`、`-3.5`、`"-5"` 分别属于什么类型？
2. 为什么 `1.0` 是 `float`，而 `1` 是 `int`？
3. `True` 和 `"True"` 的类型是否相同？
4. `None` 通常表示什么？
5. 使用 `type()` 检查 `10`、`8.5`、`"Python"`、`False` 和 `None` 的类型。

## 关联内容

- 前置知识：Python 字面量、变量、标识符。
- 后续知识：类型转换、运算符、条件判断、数据容器。
- 练习：定义五个变量，分别保存整数、小数、字符串、布尔值和空值，并打印它们的类型。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
