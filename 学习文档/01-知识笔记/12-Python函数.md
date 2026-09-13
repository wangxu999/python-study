# Python 函数

## 一句话结论

函数是将一段可重复使用的代码组织起来的工具。通过定义函数、传入参数和获得返回值，可以减少重复代码，让程序更清晰。

## 为什么使用函数

- 将重复逻辑写一次，多处调用。
- 用函数名表达代码用途，例如 `calculate_total()`。
- 将复杂任务拆分为多个较小的步骤，便于测试和修改。

## 定义和调用函数

使用 `def` 定义函数，函数体必须缩进。定义函数不会立刻执行其中的代码，只有调用函数时才会执行。

语法：

```python
def 函数名():
    函数体


函数名()
```

示例：

```python
def say_hello():
    print("你好，Python")


say_hello()
```

输出：

```text
你好，Python
```

## 函数参数

参数让同一个函数可以处理不同的数据。定义函数时写的是形参，调用函数时传入的是实参。

```python
def greet(name):
    print(f"你好，{name}！")


greet("小王")
greet("小李")
```

输出：

```text
你好，小王！
你好，小李！
```

### 位置参数

多个参数使用英文逗号分隔，调用时按参数定义的顺序传入数据。这种方式称为位置参数传递。

```python
def introduce(name, age):
    print(f"我叫{name}，今年{age}岁。")


introduce("小王", 18)
```

### 关键字参数

调用函数时可以明确写出参数名，这种方式不依赖参数顺序。

```python
introduce(age=18, name="小王")
```

位置参数和关键字参数可以混合使用，但位置参数必须写在关键字参数前面。

```python
introduce("小王", age=18)  # 正确
# introduce(name="小王", 18)  # 错误：位置参数不能写在关键字参数后面
```

### 默认参数

定义函数时可以为参数设置默认值。调用时未传入该参数，就使用默认值。

```python
def greet(name, message="你好"):
    print(f"{message}，{name}！")


greet("小王")
greet("小李", "早上好")
```

有默认值的参数通常写在没有默认值的参数后面。

```python
def introduce(name, age=18):
    print(f"我叫{name}，今年{age}岁。")
```

### 可变参数

当传入的参数数量不确定时，可以使用 `*args` 接收任意多个位置参数。函数内部的 `args` 是一个元组。

```python
def calculate_sum(*args):
    return sum(args)


print(calculate_sum(1, 2, 3))      # 6
print(calculate_sum(10, 20, 30, 40))  # 100
```

使用 `**kwargs` 可以接收任意多个关键字参数。函数内部的 `kwargs` 是一个字典。

```python
def show_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")


show_info(name="小王", age=18, city="上海")
```

`args` 和 `kwargs` 只是常用命名，可以换成其他名字；但 `*` 和 `**` 不能省略。

### 参数传递方式总结

| 方式 | 写法 | 特点 |
| --- | --- | --- |
| 位置参数 | `introduce("小王", 18)` | 按形参定义顺序传值 |
| 关键字参数 | `introduce(age=18, name="小王")` | 明确指定形参名，不依赖传入顺序 |
| 默认参数 | `def greet(name, message="你好")` | 调用时可省略有默认值的参数 |
| 可变位置参数 | `def func(*args)` | 接收任意多个位置参数，函数内为元组 |
| 可变关键字参数 | `def func(**kwargs)` | 接收任意多个关键字参数，函数内为字典 |

### Python 的对象传递特点

Python 传递参数时，会让形参引用调用处传入的对象。理解时可以记住两点：

- 在函数内为形参重新赋值，不会改变函数外部变量。
- 在函数内修改传入的可变对象，例如列表或字典，函数外部也会看到这个变化。

```python
def add_one(number):
    number += 1
    print(f"函数内：{number}")


value = 10
add_one(value)
print(f"函数外：{value}")
```

输出：

```text
函数内：11
函数外：10
```

```python
def add_course(courses):
    courses.append("Python")


course_list = ["入门"]
add_course(course_list)
print(course_list)  # ['入门', 'Python']
```

不希望函数修改原列表时，可以在调用前传入副本：`add_course(course_list.copy())`。

## lambda 匿名函数

`lambda` 用于定义一个简单的匿名函数。它通常适合只需要一行表达式、且只使用一次的小函数。

基本语法：

```python
lambda 参数1, 参数2: 表达式
```

`lambda` 会自动返回表达式的计算结果，不需要写 `return`。

```python
add = lambda a, b: a + b

print(add(3, 5))  # 8
```

上面的写法与普通函数效果相近：

```python
def add(a, b):
    return a + b
```

### 立即调用

如果匿名函数只使用一次，可以在定义后立刻调用。

```python
result = (lambda price, count: price * count)(19.9, 3)
print(result)  # 59.7
```

### 配合 sorted 使用

`lambda` 常用于给 `sorted()` 提供排序规则。例如按学生分数排序：

```python
students = [
    {"name": "小王", "score": 88},
    {"name": "小李", "score": 95},
    {"name": "小张", "score": 76},
]

sorted_students = sorted(students, key=lambda student: student["score"])

print(sorted_students)
```

这里的 `lambda student: student["score"]` 表示：每次取出一个学生字典，并使用其中的 `score` 作为排序依据。

降序排序时，可加上 `reverse=True`：

```python
sorted_students = sorted(
    students,
    key=lambda student: student["score"],
    reverse=True,
)
```

### lambda 的限制

`lambda` 的冒号后只能写一个表达式，不能写多条普通语句，例如赋值、`if` 语句、`for` 循环或 `print()` 后再写其他处理步骤。

逻辑复杂、需要多行代码或需要清晰名称时，应使用普通的 `def` 函数。

```python
def calculate_discount(price, level):
    if level == "vip":
        return price * 0.8
    return price
```

## 返回值 return

`return` 用于将函数计算的结果返回给调用处。遇到 `return` 后，函数会立即结束。

```python
def calculate_total(price, count):
    return price * count


total = calculate_total(19.9, 3)
print(total)
```

输出：

```text
59.7
```

没有写 `return` 的函数，默认返回 `None`。

```python
def show_message():
    print("提示信息")


result = show_message()
print(result)  # None
```

## 变量作用域

变量作用域指变量可以被访问和使用的范围。初学阶段主要区分全局变量和局部变量。

| 变量类型 | 定义位置 | 可访问范围 |
| --- | --- | --- |
| 全局变量 | 函数外部 | 当前文件中的函数外部和函数内部通常都可以读取 |
| 局部变量 | 函数内部 | 只能在定义它的函数内部使用 |

### 局部变量

函数内部定义的变量称为局部变量，只能在函数内部使用。

```python
def calculate():
    total = 100
    print(total)


calculate()
# print(total)  # 会报错，total 是局部变量
```

### 全局变量

函数外部定义的变量称为全局变量，在函数内部通常可以读取。

```python
tax_rate = 0.13

def calculate_tax(price):
    return price * tax_rate


print(calculate_tax(100))  # 13.0
```

### 同名变量

函数内外出现同名变量时，函数内部会优先使用局部变量；局部变量不会改变函数外的同名全局变量。

```python
name = "全局小王"

def show_name():
    name = "局部小李"
    print(name)


show_name()   # 局部小李
print(name)   # 全局小王
```

### 在函数内修改全局变量

在函数内部为一个全局变量重新赋值时，Python 会默认把它当作新的局部变量，因此会出现错误。确实需要修改时，可以使用 `global` 声明该变量是全局变量。

```python
count = 0

def add_count():
    global count
    count += 1


add_count()
print(count)  # 1
```

但应谨慎使用 `global`。多个函数都修改同一个全局变量时，程序会更难理解和排查问题。更推荐通过参数传入数据，再用 `return` 返回新结果。

```python
def add_one(count):
    return count + 1


count = 0
count = add_one(count)
print(count)  # 1
```

## 函数命名规范

- 使用小写英文字母，多个单词用下划线连接。
- 名称应表达用途，通常使用动词开头，例如 `calculate_total`、`print_report`。
- 不要使用 Python 关键字或内置名称，例如 `list`、`str`、`sum` 作为函数名。

## 易错点

- [ ] 定义函数后忘记调用。
  - 正确做法：定义后使用 `函数名()` 调用。
- [ ] 忘记在函数定义行结尾写英文冒号 `:`。
  - 正确做法：`def 函数名(...):` 必须以冒号结束。
- [ ] 函数体没有缩进。
  - 正确做法：函数体通常使用 4 个空格缩进。
- [ ] 参数数量不匹配。
  - 正确做法：调用函数时提供所需参数，或为可选参数设置默认值。
- [ ] 将位置参数写在关键字参数后面。
  - 正确做法：位置参数必须在关键字参数之前。
- [ ] 将 `*args` 当作列表，或将 `**kwargs` 当作元组。
  - 正确做法：`*args` 在函数内部是元组，`**kwargs` 在函数内部是字典。
- [ ] 在函数内修改传入的列表，却以为函数外列表不会变化。
  - 正确做法：列表是可变对象；不想修改原列表时传入 `列表.copy()`。
- [ ] 以为 `lambda` 函数需要写 `return`。
  - 正确做法：`lambda` 会自动返回冒号后表达式的结果。
- [ ] 使用 `lambda` 编写复杂的多步骤逻辑。
  - 正确做法：`lambda` 适合单个简单表达式；复杂逻辑使用 `def` 定义普通函数。
- [ ] 只用 `print()` 输出结果，却希望函数能将结果用于后续计算。
  - 正确做法：需要把结果交给后续代码时使用 `return`。
- [ ] 在函数外访问局部变量。
  - 正确做法：局部变量只能在函数内部使用；需要结果时通过 `return` 返回。
- [ ] 认为函数内部给同名变量赋值会自动修改全局变量。
  - 正确做法：函数内赋值默认创建局部变量；确实需要修改全局变量时使用 `global`，但应谨慎。
- [ ] 用大量全局变量在多个函数之间传递数据。
  - 正确做法：优先通过函数参数传入数据，并用 `return` 返回计算结果。

## 自测问题

1. 定义函数和调用函数分别使用什么语法？
2. 形参和实参有什么区别？
3. `return` 的作用是什么？
4. 没有写 `return` 的函数默认返回什么？
5. 为什么不能在函数外直接访问函数内部定义的局部变量？
6. 函数内外都有 `name` 变量时，函数内部优先使用哪个？
7. 什么情况下才需要使用 `global`？
8. 位置参数和关键字参数有什么区别？
9. `*args` 和 `**kwargs` 在函数内部的类型分别是什么？
10. 为什么函数内 `courses.append("Python")` 会影响函数外的列表？
11. `lambda a, b: a + b` 表示什么？
12. 为什么复杂逻辑通常不适合使用 `lambda`？
13. 编写一个 `calculate_sum(a, b)` 函数，返回两个数的和。

## 关联内容

- 前置知识：变量、输入输出、运算符、流程控制。
- 后续知识：类型注解、可变参数、函数文档字符串、递归、模块。
- 练习：编写一个函数，接收姓名和三门成绩，返回总分和平均分。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
