# Python 字符串

## 一句话结论

字符串用于表示文本。Python 中可以使用单引号、双引号或三引号定义字符串；单引号和双引号通常等价，三引号适合多行字符串或文档字符串。

## 三种定义方式

| 定义方式 | 写法 | 适用场景 |
| --- | --- | --- |
| 单引号 | `'Python'` | 普通单行文本 |
| 双引号 | `"Python"` | 普通单行文本，或文本中需要包含单引号 |
| 三引号 | `"""Python"""` 或 `'''Python'''` | 多行文本、函数或类的文档字符串 |

## 单引号定义字符串

使用一对英文单引号包围文本。

```python
language = 'Python'
print(language)
```

## 双引号定义字符串

使用一对英文双引号包围文本。

```python
message = "你好，Python"
print(message)
```

单引号和双引号定义的普通字符串没有本质区别，选择一种并保持代码风格一致即可。

## 三引号定义字符串

三引号可以使用三个英文双引号，也可以使用三个英文单引号。它适合保存跨越多行的文本。

```python
poem = """第一行文本
第二行文本
第三行文本"""

print(poem)
```

输出：

```text
第一行文本
第二行文本
第三行文本
```

三引号还常用于函数、类或模块开头的文档字符串。

```python
def greet(name: str) -> str:
    """返回一条欢迎信息。"""
    return f"你好，{name}"
```

## 引号嵌套

字符串内容需要包含引号时，可以让外层和内层使用不同的引号，避免转义。

```python
message1 = '他说："你好"'
message2 = "It's a book"
```

如果外层和内容中的引号相同，需要在内容前加反斜杠 `\` 进行转义。

```python
message = "他说：\"你好\""
```

## 常见转义字符

转义字符以反斜杠 `\` 开头，用来在字符串中表示特殊内容。

| 转义字符 | 名称 | 作用 |
| --- | --- | --- |
| `\'` | 单引号 | 在单引号字符串中表示单引号 `'` |
| `\"` | 双引号 | 在双引号字符串中表示双引号 `"` |
| `\n` | 换行符 | 从新的一行开始显示内容 |
| `\t` | 制表符 | 插入一个 Tab 缩进位置 |

```python
print('It\'s Python')
print("他说：\"你好\"")
print("第一行\n第二行")
print("姓名\t年龄")
print("小王\t18")
```

输出：

```text
It's Python
他说："你好"
第一行
第二行
姓名    年龄
小王    18
```

`\t` 的实际显示宽度会受编辑器、终端和当前文字位置影响，因此它适合简单分隔，不适合制作需要严格对齐的表格。

## 字符串切片

字符串切片用于获取字符串的一部分内容，返回一个新的字符串，不会修改原字符串。

基本语法：

```python
字符串[起始下标:结束下标:步长]
```

- 起始下标包含在结果中。
- 结束下标不包含在结果中。
- 步长默认是 `1`。
- 起始下标、结束下标和步长都可以省略。

```python
text = "Python"

print(text[1:4])  # yth
print(text[:3])   # Pyt
print(text[2:])   # thon
print(text[::2])  # Pto
print(text[:])    # Python
```

### 负数下标和倒序切片

负数下标从字符串末尾开始计算，`-1` 表示最后一个字符。

```python
text = "Python"

print(text[-1])    # n
print(text[-3:])   # hon
print(text[::-1])  # nohtyP
```

`[::-1]` 表示从最后一个字符开始，每次向前取一个字符，因此可用于得到倒序字符串。

### 索引和切片的区别

```python
text = "Python"

print(text[1])    # y，索引得到一个字符
print(text[1:2])  # y，切片得到一个字符串
```

索引超出范围会报错；切片超出范围通常不会报错，而是返回实际能取得的内容或空字符串。

```python
print(text[10:20])  # ''
```

字符串是不可变类型，因此不能通过下标或切片直接修改原字符串。

```python
text = "Python"
# text[0] = "J"  # 会报错，字符串中的字符不能直接修改

new_text = "J" + text[1:]
print(new_text)  # Jython
```

## 字符串常用方法

字符串是不可变类型，大部分字符串方法会返回一个新的字符串，而不会直接修改原字符串。

### 大小写转换

| 方法 | 作用 | 示例结果 |
| --- | --- | --- |
| `lower()` | 将英文字符转为小写 | `"PyThOn".lower()` 得到 `"python"` |
| `upper()` | 将英文字符转为大写 | `"PyThOn".upper()` 得到 `"PYTHON"` |
| `title()` | 将每个英文单词首字母大写 | `"hello python".title()` 得到 `"Hello Python"` |

```python
language = "PyThOn"

print(language.lower())  # python
print(language.upper())  # PYTHON
print(language)          # PyThOn，原字符串不变
```

### 查找和统计

| 方法 | 作用 | 找不到时的表现 |
| --- | --- | --- |
| `find(sub)` | 查找子字符串第一次出现的下标 | 返回 `-1` |
| `index(sub)` | 查找子字符串第一次出现的下标 | 报错 |
| `count(sub)` | 统计子字符串出现次数 | 返回 `0` |

```python
text = "Python is easy, Python is useful"

print(text.find("Python"))   # 0
print(text.find("Java"))     # -1
print(text.count("Python"))  # 2
```

### 替换和去除空白

| 方法 | 作用 | 示例结果 |
| --- | --- | --- |
| `replace(old, new)` | 将指定内容替换为新内容 | `"Python".replace("Py", "J")` 得到 `"Jthon"` |
| `strip()` | 去除两端空白字符 | `"  Python  ".strip()` 得到 `"Python"` |
| `lstrip()` | 去除左侧空白字符 | `"  Python".lstrip()` 得到 `"Python"` |
| `rstrip()` | 去除右侧空白字符 | `"Python  ".rstrip()` 得到 `"Python"` |

```python
text = "  hello Python  "

new_text = text.strip().replace("Python", "World")
print(new_text)  # hello World
```

### 拆分和连接

| 方法 | 作用 | 示例结果 |
| --- | --- | --- |
| `split(sep)` | 按分隔符拆分字符串，返回列表 | `"a,b,c".split(",")` 得到 `['a', 'b', 'c']` |
| `join(items)` | 用字符串连接多个字符串 | `"-".join(['a', 'b', 'c'])` 得到 `"a-b-c"` |

```python
date = "2026-09-12"
parts = date.split("-")

print(parts)             # ['2026', '09', '12']
print("/".join(parts))  # 2026/09/12
```

`join()` 中的所有元素都必须是字符串；例如数字列表需要先转换后再连接。

```python
numbers = [1, 2, 3]
text = ",".join(str(number) for number in numbers)
print(text)  # 1,2,3
```

### 开头、结尾和内容判断

| 方法 | 作用 | 示例 |
| --- | --- | --- |
| `startswith(prefix)` | 判断是否以指定内容开头 | `"Python.py".startswith("Py")` 得到 `True` |
| `endswith(suffix)` | 判断是否以指定内容结尾 | `"Python.py".endswith(".py")` 得到 `True` |
| `isdigit()` | 判断是否全部由数字字符组成 | `"123".isdigit()` 得到 `True` |
| `isalpha()` | 判断是否全部由字母组成 | `"Python".isalpha()` 得到 `True` |
| `isalnum()` | 判断是否全部由字母或数字组成 | `"Python314".isalnum()` 得到 `True` |

```python
file_name = "notes.txt"
user_input = "123"

print(file_name.endswith(".txt"))  # True
print(user_input.isdigit())         # True
```

## 字符串拼接

很多时候，需要将多个字符串拼接成一段完整的文本。可以直接使用加号 `+` 进行拼接。

```python
first_name = "王"
last_name = "小明"
full_name = first_name + last_name

print(full_name)
```

输出：

```text
王小明
```

也可以在字符串之间加入空格或其他文字：

```python
name = "小王"
message = "你好，" + name + "！欢迎学习 Python。"

print(message)
```

输出：

```text
你好，小王！欢迎学习 Python。
```

使用 `+` 拼接时，两边都必须是字符串。如果其中一边是数字，需要先使用 `str()` 转换为字符串。

```python
age = 18
message = "我的年龄是：" + str(age)

print(message)
```

错误示例：

```python
age = 18
message = "我的年龄是：" + age  # str 和 int 不能直接用 + 拼接
```

## %s 格式化字符串

`%s` 是字符串格式化中的占位符。先在字符串中预留 `%s` 的位置，再用 `%` 把数据填入对应位置。

基本语法：

```python
"包含 %s 的字符串" % 数据
```

### 填入一个数据

```python
name = "小王"
message = "你好，%s！" % name

print(message)
```

输出：

```text
你好，小王！
```

`%s` 可以将不同类型的数据转换为字符串后填入。

```python
age = 18
message = "我的年龄是：%s" % age

print(message)
```

输出：

```text
我的年龄是：18
```

### 填入多个数据

字符串中有多个 `%s` 时，右侧使用元组按顺序提供多个数据。

```python
name = "小王"
age = 18
message = "我叫%s，今年%s岁。" % (name, age)

print(message)
```

输出：

```text
我叫小王，今年18岁。
```

占位符数量和提供的数据数量必须一致，并且数据填入顺序从左到右对应。

## f-string 格式化字符串

f-string 是更常用、更直观的字符串格式化方式。在字符串开头加 `f` 或 `F`，再用花括号 `{}` 包围需要填入的变量或表达式。

基本语法：

```python
f"文本内容 {变量或表达式}"
```

### 填入变量

```python
name = "小王"
age = 18
message = f"我叫{name}，今年{age}岁。"

print(message)
```

输出：

```text
我叫小王，今年18岁。
```

f-string 可以直接处理数字，不需要像 `+` 拼接那样手动调用 `str()`。

### 在花括号中计算表达式

花括号中不仅可以写变量，也可以写简单表达式。

```python
price = 19.9
count = 3

message = f"总价是：{price * count} 元"
print(message)
```

输出：

```text
总价是：59.7 元
```

### 保留小数位数

浮点数可以使用 `:.2f` 保留两位小数。

```python
average = 89.456
print(f"平均分：{average:.2f}")
```

输出：

```text
平均分：89.46
```

## 三种拼接和格式化方式对比

| 方式 | 示例 | 特点 |
| --- | --- | --- |
| `+` 拼接 | `"年龄：" + str(age)` | 简单文本可用，数字需先转换为字符串 |
| `%s` 格式化 | `"年龄：%s" % age` | 旧式写法，学习旧代码时常见 |
| f-string | `f"年龄：{age}"` | 直观易读，推荐在新代码中优先使用 |

## 易错点
## 易错点
## 易错点

- [ ] 使用中文引号 `“”` 或 `‘’`。
  - 正确做法：Python 代码中使用英文半角引号 `'` 或 `"`。
- [ ] 字符串开头和结尾使用不同类型的引号。
  - 错误示例：`name = 'Python"`
  - 正确做法：开头和结尾必须配对。
- [ ] 以为三引号只是普通注释。
  - 正确做法：三引号本质是字符串；在函数、类或模块开头时通常作为文档字符串。
- [ ] 在单引号字符串中直接再写单引号。
  - 正确做法：改用双引号作为外层，或使用反斜杠转义。
- [ ] 将 `\n` 或 `\t` 写成 `/n` 或 `/t`。
  - 正确做法：转义字符使用反斜杠 `\`，不是正斜杠 `/`。
- [ ] 想显示反斜杠本身，却只写一个反斜杠。
  - 正确做法：使用 `\\` 表示一个反斜杠，例如 `"C:\\Python"`。
- [ ] 以为切片的结束下标也会被取到。
  - 正确做法：切片遵循“左闭右开”，`text[1:4]` 取下标 `1`、`2`、`3`。
- [ ] 将字符串索引结果和切片结果混淆。
  - 正确做法：`text[1]` 得到一个字符，`text[1:2]` 得到一个字符串。
- [ ] 试图通过 `text[0] = "J"` 修改字符串。
  - 正确做法：字符串不可变，需要组合出新字符串。
- [ ] 认为 `lower()`、`replace()`、`strip()` 会直接修改原字符串。
  - 正确做法：将返回的新字符串保存到变量，例如 `new_text = text.strip()`。
- [ ] 用 `index()` 判断子字符串是否存在。
  - 正确做法：不确定内容是否存在时，使用 `find()` 或 `in`，避免 `index()` 找不到时抛出错误。
- [ ] 直接用 `join()` 连接整数列表。
  - 正确做法：先用 `str()` 将每个数字转换为字符串。
- [ ] 直接用 `+` 拼接字符串和数字。
  - 正确做法：先用 `str()` 将数字转换为字符串，或在后续学习中使用 f-string 格式化。
- [ ] `%s` 的数量和右侧数据数量不一致。
  - 正确做法：每个 `%s` 都要有一个对应的数据。
- [ ] 多个数据时忘记使用元组。
  - 正确写法：`"%s 和 %s" % ("A", "B")`。
- [ ] 忘记在字符串开头加 `f`。
  - 错误示例：`"我叫{name}"`，它会原样输出 `{name}`。
  - 正确做法：`f"我叫{name}"`。
- [ ] 在 f-string 中使用圆括号或方括号代替花括号。
  - 正确做法：变量和表达式必须写在花括号 `{}` 中。

## 自测问题

1. Python 定义字符串有哪三种方式？
2. 单引号和双引号定义普通字符串有什么区别？
3. 多行文本适合使用哪种定义方式？
4. 如何写出包含双引号的字符串 `他说：“你好”`？
5. 三引号在函数开头通常有什么用途？
6. `\n` 和 `\t` 分别有什么作用？
7. `text[1:4]` 是否包含下标为 `4` 的字符？
8. `text[::-1]` 的作用是什么？
9. 为什么不能使用 `text[0] = "J"` 修改字符串？
10. `find()` 和 `index()` 在找不到内容时有什么区别？
11. 如何将 `"2026-09-12"` 转为 `"2026/09/12"`？
12. 为什么 `"年龄：" + 18` 会报错？应如何修改？
13. 使用 `%s` 格式化输出姓名和年龄。
14. 使用 f-string 输出姓名、年龄和两位小数的平均分。

## 关联内容

- 前置知识：Python 字面量、常见数据类型、Python 注释。
- 后续知识：字符串常用方法。
- 练习：分别用单引号、双引号和三引号定义同一段文本，并打印结果。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
