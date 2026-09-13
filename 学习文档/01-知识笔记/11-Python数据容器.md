# Python 数据容器

## 一句话结论

数据容器用于存储多个数据。Python 中常见的数据容器有列表 `list`、元组 `tuple`、集合 `set` 和字典 `dict`。

补充：字符串 `str` 从广义上也可以看作容器或序列，因为它由多个字符组成，支持遍历、索引和 `in` 判断；但入门学习中通常将它单独作为文本类型讲解。

## 常见数据容器总览

| 容器类型 | 标准名称 | 基本写法 | 主要特点 |
| --- | --- | --- | --- |
| 列表 | `list` | `['Python', 'Java']` | 有顺序、可修改、可存重复数据 |
| 元组 | `tuple` | `('上海', '北京')` | 有顺序、通常不可修改、可存重复数据 |
| 集合 | `set` | `{'Python', 'Java'}` | 无固定顺序、元素不重复 |
| 字典 | `dict` | `{'name': '小王', 'age': 18}` | 使用键值对保存数据，通过键查找值 |

## 数据容器总结与对比

| 特性 | 字符串 `str` | 列表 `list` | 元组 `tuple` | 集合 `set` | 字典 `dict` |
| --- | --- | --- | --- | --- | --- |
| 有序性 | 有序 | 有序 | 有序 | 无固定顺序 | 有序，Python 3.7 起保持键的插入顺序 |
| 重复元素 | 允许重复字符 | 允许重复元素 | 允许重复元素 | 不允许重复元素 | 键不允许重复，值可以重复 |
| 可变性 | 不可变 | 可变 | 不可变 | 可变 | 可变 |
| 索引访问 | 支持 | 支持 | 支持 | 不支持 | 不支持按下标索引，按键访问 |
| 切片操作 | 支持 | 支持 | 支持 | 不支持 | 不支持 |
| 常见使用场景 | 文本处理 | 有序且可重复的数据集合 | 固定数据记录 | 去重数据集合 | 键值对数据 |

补充说明：字典的顺序指键值对的插入顺序，不是按键名或值自动排序。虽然字典不能按数字下标访问，但可以通过键访问对应的值，例如 `student["name"]`。

## 基本示例

```python
names = ["小王", "小李", "小张"]
point = (10, 20)
tags = {"Python", "入门", "Python"}
student = {"name": "小王", "age": 18}

print(names)
print(point)
print(tags)
print(student)
```

可能输出：

```text
['小王', '小李', '小张']
(10, 20)
{'Python', '入门'}
{'name': '小王', 'age': 18}
```

集合是无序的，因此每次显示元素的顺序可能不同；重复的 `"Python"` 只会保留一份。

## 列表切片

列表切片用于获取列表中的一部分元素，不会修改原列表，而是返回一个新的列表。

基本语法：

```python
列表[起始下标:结束下标:步长]
```

- 起始下标包含在结果中。
- 结束下标不包含在结果中。
- 步长默认是 `1`。
- 三个部分都可以省略。

```python
names = ["小王", "小李", "小张", "小赵", "小陈"]

print(names[1:4])   # ['小李', '小张', '小赵']
print(names[:3])    # ['小王', '小李', '小张']
print(names[2:])    # ['小张', '小赵', '小陈']
print(names[::2])   # ['小王', '小张', '小陈']
print(names[:])     # 复制整个列表
```

### 负数下标和倒序切片

负数下标从列表末尾开始计算，`-1` 表示最后一个元素。

```python
names = ["小王", "小李", "小张", "小赵", "小陈"]

print(names[-1])    # 小陈
print(names[-3:])   # ['小张', '小赵', '小陈']
print(names[::-1])  # ['小陈', '小赵', '小张', '小李', '小王']
```

`[::-1]` 表示从最后一个元素开始，每次向前取一个元素，因此可以得到倒序的新列表。

### 切片和索引的区别

```python
names = ["小王", "小李", "小张"]

print(names[1])    # '小李'，索引得到一个元素
print(names[1:2])  # ['小李']，切片得到一个列表
```

索引超出范围会报错；切片超出范围通常不会报错，而是返回实际能取得的部分或空列表。

```python
print(names[10:20])  # []
```

## 元组切片

元组和列表一样都是有序序列，因此也支持索引和切片。元组切片会返回一个新的元组，不会修改原元组。

基本语法：

```python
元组[起始下标:结束下标:步长]
```

```python
cities = ("上海", "北京", "广州", "深圳", "杭州")

print(cities[1:4])   # ('北京', '广州', '深圳')
print(cities[:3])    # ('上海', '北京', '广州')
print(cities[2:])    # ('广州', '深圳', '杭州')
print(cities[::2])   # ('上海', '广州', '杭州')
print(cities[::-1])  # ('杭州', '深圳', '广州', '北京', '上海')
```

负数下标同样从元组末尾开始计算：

```python
print(cities[-1])   # 杭州
print(cities[-3:])  # ('广州', '深圳', '杭州')
```

元组是不可变类型，不能通过下标或切片修改其中的元素。

```python
point = (10, 20)
# point[0] = 100  # 会报错，元组元素不能直接修改
```

## 列表常用方法

列表方法通过 `列表.方法名()` 的形式调用。大部分增删改排序方法会直接修改原列表。

### 添加元素

| 方法 | 作用 | 示例 | 结果 |
| --- | --- | --- | --- |
| `append(x)` | 在列表末尾添加一个元素 | `names.append("小赵")` | 添加一个元素 |
| `insert(i, x)` | 在下标 `i` 前插入元素 | `names.insert(1, "小李")` | 指定位置添加一个元素 |
| `extend(items)` | 将另一个可迭代对象的元素逐个追加 | `names.extend(["小赵", "小陈"])` | 一次添加多个元素 |

```python
names = ["小王", "小张"]

names.append("小赵")
names.insert(1, "小李")
names.extend(["小陈", "小刘"])

print(names)
```

输出：

```text
['小王', '小李', '小张', '小赵', '小陈', '小刘']
```

`append()` 会把整个对象作为一个元素加入列表；`extend()` 会逐个加入其中的元素。

```python
numbers = [1, 2]
numbers.append([3, 4])
print(numbers)  # [1, 2, [3, 4]]

numbers = [1, 2]
numbers.extend([3, 4])
print(numbers)  # [1, 2, 3, 4]
```

### 删除元素

| 方法 | 作用 | 返回值 |
| --- | --- | --- |
| `remove(x)` | 删除第一个值为 `x` 的元素 | `None` |
| `pop()` | 删除并返回最后一个元素 | 被删除的元素 |
| `pop(i)` | 删除并返回下标为 `i` 的元素 | 被删除的元素 |
| `clear()` | 删除所有元素 | `None` |

```python
names = ["小王", "小李", "小张", "小李"]

names.remove("小李")
last_name = names.pop()
first_name = names.pop(0)

print(names)       # ['小张']
print(last_name)   # 小李
print(first_name)  # 小王
```

`remove()` 只删除第一个匹配值。如果要删除的元素不存在，会报错；`pop(i)` 的下标超出范围也会报错。

### 查询元素

| 方法或写法 | 作用 | 示例 |
| --- | --- | --- |
| `index(x)` | 获取第一个值为 `x` 的元素下标 | `names.index("小李")` |
| `count(x)` | 统计值为 `x` 的元素出现次数 | `names.count("小李")` |
| `x in names` | 判断元素是否存在 | `"小李" in names` |

```python
names = ["小王", "小李", "小张", "小李"]

print(names.index("小李"))  # 1
print(names.count("小李"))  # 2
print("小赵" in names)      # False
```

### 排序和反转

| 方法 | 作用 | 示例 |
| --- | --- | --- |
| `sort()` | 按升序排序原列表 | `numbers.sort()` |
| `sort(reverse=True)` | 按降序排序原列表 | `numbers.sort(reverse=True)` |
| `reverse()` | 直接反转原列表元素顺序 | `numbers.reverse()` |

```python
numbers = [3, 1, 4, 2]

numbers.sort()
print(numbers)  # [1, 2, 3, 4]

numbers.reverse()
print(numbers)  # [4, 3, 2, 1]
```

`sort()` 和 `reverse()` 都直接修改原列表，返回值是 `None`。如果想保留原列表，可使用 `sorted(列表)` 得到排序后的新列表。

```python
numbers = [3, 1, 4, 2]
new_numbers = sorted(numbers)

print(numbers)      # [3, 1, 4, 2]
print(new_numbers)  # [1, 2, 3, 4]
```

### 复制列表

`copy()` 可以创建列表的浅拷贝。

```python
names = ["小王", "小李"]
new_names = names.copy()

new_names.append("小张")

print(names)      # ['小王', '小李']
print(new_names)  # ['小王', '小李', '小张']
```

## 列表常用内置函数

下面四个函数不是列表的方法，因此写法是 `函数名(列表)`，而不是 `列表.函数名()`。它们也常用于元组、字符串等其他可迭代对象。

| 函数 | 作用 | 示例 | 结果 |
| --- | --- | --- | --- |
| `len()` | 获取元素数量 | `len(numbers)` | 列表长度 |
| `sum()` | 计算所有数值元素之和 | `sum(numbers)` | 数值总和 |
| `max()` | 获取最大元素 | `max(numbers)` | 最大值 |
| `min()` | 获取最小元素 | `min(numbers)` | 最小值 |

```python
numbers = [88, 92, 76, 100]

print(len(numbers))  # 4
print(sum(numbers))  # 356
print(max(numbers))  # 100
print(min(numbers))  # 76
```

### 计算平均值

可以将 `sum()` 和 `len()` 组合，计算一组数的平均值。

```python
scores = [88, 92, 76, 100]
average = sum(scores) / len(scores)

print(average)  # 89.0
```

列表为空时，`len([])` 和 `sum([])` 可以正常得到 `0`，但 `max([])` 和 `min([])` 会报错，因为空列表中没有可比较的元素。

```python
numbers = []

print(len(numbers))  # 0
print(sum(numbers))  # 0

if numbers:
    print(max(numbers))
else:
    print("列表为空，无法获取最大值")
```

`sum()` 适用于数值列表，不能用于字符串列表。

```python
names = ["小王", "小李"]
# sum(names)  # 会报错，字符串不能使用 sum() 求和
```

## 元组组包与解包

组包也常称为打包，指将多个值组合为一个元组；解包指将元组中的多个元素依次赋值给多个变量。

### 组包

多个值用英文逗号隔开时，Python 会将它们组包成元组。圆括号可以省略，但建议在表达复杂时加上圆括号以提高可读性。

```python
point = 10, 20
name, age = "小王", 18

print(point)        # (10, 20)
print(type(point))  # <class 'tuple'>
```

创建只包含一个元素的元组时，元素后面必须加逗号，否则 Python 会将其视为普通值。

```python
number = (10)   # int，不是元组
single = (10,)  # tuple，单元素元组
```

### 基本解包

解包时，左侧变量数量通常要与右侧元组中的元素数量一致。

```python
point = (10, 20)
x, y = point

print(x)  # 10
print(y)  # 20
# first, second, third = point  # 会报错：右侧只有 2 个元素，左侧有 3 个变量
```

上面的字符串示例说明：元素数量不一致时会报错。正确写法应让左侧变量数量与字符数量一致，或使用 `*` 接收剩余元素。

### 使用星号接收剩余元素

在一个变量名前加 `*`，可以接收剩余的多个元素，并将它们保存为列表。

```python
numbers = (10, 20, 30, 40, 50)

first, *middle, last = numbers

print(first)   # 10
print(middle)  # [20, 30, 40]
print(last)    # 50
```

`*` 接收变量可以放在左侧、中间或右侧，但一次解包中通常只能有一个带 `*` 的变量。

```python
first, *others = (10, 20, 30)
print(first)   # 10
print(others)  # [20, 30]
```

### 交换变量的值

Python 可以利用组包与解包，在不使用临时变量的情况下交换两个变量的值。

```python
a = 10
b = 20

a, b = b, a

print(a)  # 20
print(b)  # 10
```

右侧会先组包为一个临时元组，再解包赋值给左侧变量。

## 集合常见操作

集合 `set` 用于存储不重复的元素，元素没有固定顺序，因此不能使用下标索引或切片。集合特别适合去重、成员判断和集合关系计算。

### 创建集合

```python
tags = {"Python", "入门", "Python"}
empty_tags = set()

print(tags)         # 包含 "Python" 和 "入门"，重复值会自动去除
print(empty_tags)   # set()
```

`{}` 创建的是空字典；创建空集合必须使用 `set()`。

### 添加元素

| 方法 | 作用 | 示例 |
| --- | --- | --- |
| `add(x)` | 添加一个元素 | `tags.add("基础")` |
| `update(items)` | 添加多个元素 | `tags.update(["函数", "容器"])` |

```python
tags = {"Python", "入门"}

tags.add("基础")
tags.update(["函数", "容器"])
tags.add("Python")  # 已存在的元素不会重复添加

print(tags)
```

### 删除元素

| 方法 | 作用 | 元素不存在时 |
| --- | --- | --- |
| `remove(x)` | 删除指定元素 | 报错 |
| `discard(x)` | 删除指定元素 | 不报错 |
| `pop()` | 删除并返回一个任意元素 | 空集合时会报错 |
| `clear()` | 清空所有元素 | 不报错 |

```python
tags = {"Python", "入门", "基础"}

tags.remove("入门")
tags.discard("不存在的标签")
removed_tag = tags.pop()

print(removed_tag)  # 被删除的任意一个元素
print(tags)
```

不确定元素是否存在时，优先使用 `discard()`；需要确认元素存在时再使用 `remove()`。

### 成员判断和元素数量

```python
tags = {"Python", "入门", "基础"}

print("Python" in tags)       # True
print("Java" not in tags)     # True
print(len(tags))               # 3
```

集合适合做成员判断，因为它不需要像列表那样按顺序逐个查找。

### 集合运算

```python
python_students = {"小王", "小李", "小张"}
java_students = {"小李", "小张", "小赵"}
```

| 运算 | 写法 | 说明 | 示例结果 |
| --- | --- | --- | --- |
| 并集 | `a | b` 或 `a.union(b)` | 合并两个集合并去重 | `{'小王', '小李', '小张', '小赵'}` |
| 交集 | `a & b` 或 `a.intersection(b)` | 两个集合共有的元素 | `{'小李', '小张'}` |
| 差集 | `a - b` 或 `a.difference(b)` | 在 `a` 中但不在 `b` 中的元素 | `{'小王'}` |
| 对称差集 | `a ^ b` 或 `a.symmetric_difference(b)` | 只在其中一个集合中的元素 | `{'小王', '小赵'}` |

```python
print(python_students | java_students)
print(python_students & java_students)
print(python_students - java_students)
print(python_students ^ java_students)
```

集合的显示顺序不固定，上表中的结果只表示元素内容，不表示实际打印顺序。

### 子集和超集判断

```python
basic_tags = {"Python", "入门"}
all_tags = {"Python", "入门", "函数"}

print(basic_tags <= all_tags)  # True，basic_tags 是 all_tags 的子集
print(all_tags >= basic_tags)  # True，all_tags 是 basic_tags 的超集
```

## 字典常见操作

字典 `dict` 使用“键: 值”的形式保存数据。键通常用字符串、整数或元组等不可变值；同一个字典中的键不能重复，值可以重复。

### 创建和访问字典

```python
student = {
    "name": "小王",
    "age": 18,
    "score": 95,
}

print(student["name"])  # 小王
print(student["age"])   # 18
```

使用 `字典[键]` 访问不存在的键会报错。若不确定键是否存在，使用 `get()` 更安全。

```python
print(student.get("score"))              # 95
print(student.get("city"))               # None
print(student.get("city", "未填写"))     # 未填写
```

### 添加和修改键值对

为不存在的键赋值会添加新键值对；为已存在的键赋值会修改对应的值。

```python
student = {"name": "小王", "age": 18}

student["city"] = "上海"  # 添加
student["age"] = 19        # 修改

print(student)
```

`update()` 可以一次添加或修改多个键值对。

```python
student.update({"score": 95, "age": 20})
print(student)
```

### 删除键值对

| 方法或语句 | 作用 | 返回值或特点 |
| --- | --- | --- |
| `pop(key)` | 删除指定键并返回对应值 | 键不存在会报错 |
| `pop(key, default)` | 删除指定键，键不存在时返回默认值 | 不报错 |
| `popitem()` | 删除并返回最后添加的一组键值对 | 空字典时会报错 |
| `clear()` | 清空所有键值对 | 字典变为空字典 |
| `del dict[key]` | 删除指定键值对 | 键不存在会报错 |

```python
student = {"name": "小王", "age": 18, "city": "上海"}

age = student.pop("age")
student.pop("score", None)
del student["city"]

print(age)      # 18
print(student)  # {'name': '小王'}
```

### 获取键、值和键值对

| 方法 | 作用 |
| --- | --- |
| `keys()` | 获取所有键 |
| `values()` | 获取所有值 |
| `items()` | 获取所有键值对 |

```python
student = {"name": "小王", "age": 18, "score": 95}

print(student.keys())
print(student.values())
print(student.items())
print(len(student))  # 3，键值对数量
```

### 遍历字典

直接遍历字典默认得到键；需要同时得到键和值时，使用 `items()`。

```python
student = {"name": "小王", "age": 18, "score": 95}

for key in student:
    print(key)

for key, value in student.items():
    print(f"{key}: {value}")
```

### 成员判断

`in` 用于判断键是否存在，而不是判断值是否存在。

```python
student = {"name": "小王", "age": 18}

print("name" in student)          # True，判断键
print("小王" in student)           # False，不直接判断值
print("小王" in student.values())  # True，判断值
```

## 如何选择容器
## 如何选择容器
## 如何选择容器
## 如何选择容器

- 需要按顺序保存多项数据，并且后续可能增删改时，使用列表。
- 数据顺序固定且通常不希望被修改时，使用元组。
- 需要去重，或判断某个元素是否存在时，使用集合。
- 需要为数据取名称，例如姓名、年龄、成绩时，使用字典。

## 易错点

- [ ] 将 `[]`、`()`、`{}` 的用途混淆。
  - 正确做法：列表用 `[]`，元组用 `()`，集合和字典用 `{}`。
- [ ] 以为集合会保留添加时的固定顺序。
  - 正确做法：集合没有固定顺序，不要依赖其显示顺序。
- [ ] 以为字典和集合都只存单个元素。
  - 正确做法：字典用 `键: 值` 保存键值对；集合只保存元素本身。
- [ ] 以为切片的结束下标也会被取到。
  - 正确做法：切片遵循“左闭右开”，`names[1:4]` 取下标 `1`、`2`、`3`。
- [ ] 以为 `names[:]` 会得到原列表本身。
  - 正确做法：它会创建一个新的浅拷贝列表。
- [ ] 将索引结果和切片结果混淆。
  - 正确做法：`names[1]` 是单个元素，`names[1:2]` 是包含一个元素的新列表。
- [ ] 以为元组不能切片，因为元组不可变。
  - 正确做法：元组不能修改元素，但可以切片；切片会生成新元组。
- [ ] 将 `append()` 和 `extend()` 混淆。
  - 正确做法：`append()` 添加一个整体元素，`extend()` 逐个添加多个元素。
- [ ] 以为 `remove()` 会删除所有相同元素。
  - 正确做法：`remove()` 只删除第一个匹配元素。
- [ ] 使用 `new_list = old_list.sort()` 获取排序结果。
  - 正确做法：`sort()` 返回 `None` 并修改原列表；需要新列表时使用 `sorted(old_list)`。
- [ ] 对空列表直接使用 `max()` 或 `min()`。
  - 正确做法：先判断列表是否为空，或提供默认值后再处理。
- [ ] 用 `sum()` 拼接字符串。
  - 正确做法：`sum()` 用于数值求和；字符串拼接使用 `+`、`join()` 或 f-string。
- [ ] 解包时左侧变量数量与右侧元素数量不一致。
  - 正确做法：让数量一致，或使用一个带 `*` 的变量接收剩余元素。
- [ ] 以为 `*middle` 接收的是元组。
  - 正确做法：解包时带 `*` 的变量接收结果是列表。
- [ ] 以为 `a, b = b, a` 会先覆盖 `a`，导致无法交换。
  - 正确做法：Python 会先计算并组包右侧值，再一次性解包赋值给左侧。
- [ ] 使用下标访问集合，例如 `tags[0]`。
  - 正确做法：集合无固定顺序，不支持索引和切片。
- [ ] 用 `{}` 创建空集合。
  - 正确做法：空集合使用 `set()`，`{}` 是空字典。
- [ ] 不确定元素是否存在时使用 `remove()`。
  - 正确做法：不确定时使用 `discard()`，避免元素不存在导致报错。
- [ ] 依赖集合打印时的元素顺序。
  - 正确做法：集合无固定顺序；如需顺序，使用列表或对集合调用 `sorted()`。
- [ ] 使用 `student["不存在的键"]` 读取不确定是否存在的键。
  - 正确做法：使用 `student.get("键", 默认值)` 安全读取。
- [ ] 以为字典遍历时默认得到值。
  - 正确做法：直接遍历字典得到键；同时获取键和值时使用 `items()`。
- [ ] 以为 `"小王" in student` 会检查字典中的值。
  - 正确做法：`in` 默认检查键；检查值时使用 `"小王" in student.values()`。
- [ ] 使用可变对象例如列表作为字典键。
  - 正确做法：字典键必须是不可变且可哈希的对象；初学阶段优先使用字符串或整数作为键。

## 自测问题

1. Python 中四种常见数据容器分别是什么？
2. 需要保存多个允许重复的学生姓名，应使用什么容器？
3. 需要去除重复标签，应使用什么容器？
4. 为什么 `{}` 不是空集合？
5. 如何用字典保存姓名为“小王”、年龄为 18 的学生信息？
6. `names[1:4]` 是否包含下标为 `4` 的元素？
7. `names[::-1]` 的作用是什么？
8. `cities[1:4]` 是否包含下标为 `4` 的元素？
9. 元组为什么可以切片，却不能使用 `point[0] = 100` 修改元素？
10. `append()` 和 `extend()` 的区别是什么？
11. `pop()` 和 `remove()` 的区别是什么？
12. 如何在不修改原列表的情况下得到升序排序的新列表？
13. 如何计算列表中所有分数的平均值？
14. 为什么 `max([])` 会报错？
15. `x, y = (10, 20)` 执行后，`x` 和 `y` 分别是什么？
16. `first, *others = (1, 2, 3)` 中，`others` 的值和类型是什么？
17. 如何使用一行代码交换 `a` 和 `b` 的值？
18. `add()` 和 `update()` 的区别是什么？
19. `remove()` 和 `discard()` 的区别是什么？
20. 如何得到两个集合共有的元素？
21. `student["name"]` 和 `student.get("name")` 都可以做什么？
22. 如何安全读取可能不存在的 `"city"` 键，并在缺失时返回“未填写”？
23. 如何同时遍历字典的键和值？

## 关联内容

- 前置知识：字面量、变量、数据类型、`for` 循环。
- 后续知识：列表、元组、集合、字典的增删改查和遍历。
- 练习：分别创建一个列表、元组、集合和字典，并使用 `type()` 查看它们的类型。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
