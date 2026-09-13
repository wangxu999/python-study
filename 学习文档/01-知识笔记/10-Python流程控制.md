# Python 流程控制

## 一句话结论

流程控制用于决定程序代码的执行顺序。条件分支会根据条件表达式的结果，选择执行不同的代码；条件结果为 `True` 时执行对应代码块，为 `False` 时跳过或进入其他分支。

## 条件表达式

`if` 后面需要写一个结果为 `True` 或 `False` 的条件表达式。常用条件由比较运算符和逻辑运算符组成。

```python
age >= 18
score >= 60 and is_present
not is_closed
```

详细的比较运算符和逻辑运算符见 [Python运算符](09-Python运算符.md)。

## if 语句

当条件成立时，执行缩进的代码块。

语法：

```python
if 条件:
    条件成立时执行的代码
```

示例：

```python
score = 90

if score >= 60:
    print("成绩合格")
```

输出：

```text
成绩合格
```

## if else 语句

当条件成立时执行 `if` 代码块；不成立时执行 `else` 代码块。

语法：

```python
if 条件:
    条件成立时执行的代码
else:
    条件不成立时执行的代码
```

示例：

```python
score = 55

if score >= 60:
    print("成绩合格")
else:
    print("成绩不合格")
```

输出：

```text
成绩不合格
```

## if elif else 语句

当有多个条件需要依次判断时，使用 `elif` 增加分支。程序会从上到下判断，遇到第一个成立的条件后执行对应代码，并跳过后续分支。

语法：

```python
if 条件1:
    条件1成立时执行的代码
elif 条件2:
    条件2成立时执行的代码
else:
    以上条件都不成立时执行的代码
```

示例：

```python
score = 85

if score >= 90:
    print("优秀")
elif score >= 60:
    print("合格")
else:
    print("不合格")
```

输出：

```text
合格
```

## match case 匹配

`match...case` 用于将一个值与多个模式依次匹配，并执行第一个匹配成功的分支。它适合“根据固定选项选择不同处理”的场景。

`match...case` 从 Python 3.10 开始支持；当前安装的 Python 3.14 可以直接使用。

基本语法：

```python
match 要匹配的值:
    case 模式1:
        模式1匹配时执行的代码
    case 模式2:
        模式2匹配时执行的代码
    case _:
        以上模式都不匹配时执行的代码
```

### 基本示例

```python
command = "start"

match command:
    case "start":
        print("开始程序")
    case "stop":
        print("停止程序")
    case "restart":
        print("重启程序")
    case _:
        print("未知命令")
```

输出：

```text
开始程序
```

### 默认分支 case 下划线

`case _` 表示“其他所有情况”，作用类似 `if...elif...else` 中的 `else`。它通常写在最后。

```python
day = 8

match day:
    case 1:
        print("星期一")
    case 2:
        print("星期二")
    case _:
        print("暂未定义的日期")
```

### 匹配多个值

使用竖线 `|` 可以在一个 `case` 中匹配多个固定值，含义是“或”。

```python
day = 6

match day:
    case 1 | 2 | 3 | 4 | 5:
        print("工作日")
    case 6 | 7:
        print("周末")
    case _:
        print("日期无效")
```

输出：

```text
周末
```

### match case 和 if elif 的选择

| 场景 | 推荐写法 |
| --- | --- |
| 判断范围，例如分数是否大于等于 60 | `if...elif...else` |
| 判断多个固定值，例如命令、菜单选项、星期编号 | `match...case` |
| 需要同时判断多个复杂条件 | `if...elif...else` 配合 `and`、`or` |

## while 循环

`while` 循环会在条件为 `True` 时重复执行代码块；每次执行完成后，Python 会再次判断条件。条件变为 `False` 时，循环结束。

基本语法：

```python
while 条件:
    条件成立时重复执行的代码
```

### 计数循环示例

```python
count = 1

while count <= 3:
    print(f"当前是第 {count} 次")
    count += 1
```

输出：

```text
当前是第 1 次
当前是第 2 次
当前是第 3 次
```

`count += 1` 非常重要，它会让 `count` 逐步增加，最终使 `count <= 3` 变为 `False`，循环才会停止。

### 使用 while 累加

```python
number = 1
total = 0

while number <= 5:
    total += number
    number += 1

print(total)
```

输出：

```text
15
```

### 使用 break 提前结束循环

`break` 用于立刻结束当前的整个循环。

```python
while True:
    command = input("请输入 quit 退出：")

    if command == "quit":
        break

    print(f"你输入的是：{command}")

print("程序结束")
```

这里的 `while True` 会一直循环，直到用户输入 `quit` 并执行 `break`。

### while else

当 `while` 因为条件变为 `False` 而正常结束时，可以执行 `else` 代码块。若循环中执行了 `break`，则不会执行 `else`。

```python
count = 1

while count <= 3:
    print(count)
    count += 1
else:
    print("循环正常结束")
```

## for 循环

`for` 循环用于依次遍历一个可迭代对象中的每个元素，例如字符串、列表和 `range()` 生成的数字序列。已知需要遍历的内容或次数时，通常优先使用 `for` 循环。

基本语法：

```python
for 变量 in 可迭代对象:
    对每个元素重复执行的代码
```

### 遍历字符串

字符串由多个字符组成，`for` 可以依次取出每个字符。

```python
word = "Python"

for character in word:
    print(character)
```

输出：

```text
P
y
t
h
o
n
```

### 遍历列表

```python
names = ["小王", "小李", "小张"]

for name in names:
    print(f"你好，{name}")
```

### 使用 range 指定次数

`range()` 常用于按次数循环。`range(起始值, 结束值)` 包含起始值，但不包含结束值。

```python
for number in range(1, 6):
    print(number)
```

输出：

```text
1
2
3
4
5
```

`range(5)` 等效于 `range(0, 5)`，会生成 `0` 到 `4`。

```python
for number in range(5):
    print(number)
```

`range()` 还可以指定步长：`range(起始值, 结束值, 步长)`。

```python
for number in range(0, 10, 2):
    print(number)
```

输出：

```text
0
2
4
6
8
```

### 使用 for 累加

```python
total = 0

for number in range(1, 6):
    total += number

print(total)
```

输出：

```text
15
```

### for 和 while 的选择

| 场景 | 推荐循环 |
| --- | --- |
| 已知需要执行的次数 | `for` 配合 `range()` |
| 需要逐个处理字符串、列表等数据 | `for` |
| 不知道具体次数，只要条件满足就继续 | `while` |
| 需要等待用户输入特定内容后结束 | `while` 配合 `break` |

## 缩进规则
## 缩进规则
## 缩进规则

Python 使用缩进表示代码块范围。`if`、`elif`、`else` 结尾的冒号 `:` 不能省略，代码块通常使用 4 个空格缩进。

```python
age = 18

if age >= 18:
    print("成年人")
    print("可以继续下一步")

print("流程结束")
```

只有缩进的两行属于 `if` 代码块；最后一行不属于 `if`，无论条件是否成立都会执行。

## 综合示例

```python
age = int(input("请输入年龄："))
has_ticket = input("是否有车票 True 或 False：") == "True"

if age >= 18 and has_ticket:
    print("可以进入")
elif age < 18:
    print("年龄不符合要求")
else:
    print("请先购买车票")
```

## 易错点

- [ ] 忘记在 `if`、`elif`、`else` 末尾写英文冒号 `:`。
  - 正确做法：每个分支语句末尾都要有 `:`。
- [ ] 没有正确缩进代码块。
  - 正确做法：同一代码块保持一致缩进，建议使用 4 个空格。
- [ ] 把 `elif` 写成 `else if`。
  - 正确做法：Python 使用 `elif`。
- [ ] 将 `=` 用在判断条件中。
  - 正确做法：判断相等使用 `==`，赋值才使用 `=`。
- [ ] 将范围较大的条件放在前面，导致后续分支永远无法执行。
  - 正确做法：多个 `elif` 通常从范围更严格的条件开始写。
- [ ] 忘记在 `match` 和每个 `case` 后面写英文冒号 `:`。
  - 正确做法：`match 值:` 和 `case 模式:` 都需要冒号。
- [ ] 将默认分支写成 `case else`。
  - 正确做法：`match...case` 的默认分支写为 `case _:`。
- [ ] 以为每个 `case` 后需要写 `break`。
  - 正确做法：Python 的 `match...case` 在执行第一个匹配分支后不会自动继续匹配，不需要写 `break`。
- [ ] 循环条件永远为 `True`，且循环体内没有结束方式。
  - 正确做法：更新循环变量使条件最终变为 `False`，或在适当条件下使用 `break`。
- [ ] 使用其他语言的 `count++` 来增加计数器。
  - 正确做法：Python 使用 `count += 1`，不支持 `count++`。
- [ ] 忘记缩进 `while` 循环体。
  - 正确做法：循环体必须保持一致缩进，通常使用 4 个空格。
- [ ] 认为 `range(1, 6)` 会生成 `1` 到 `6`。
  - 正确做法：结束值不包含在内，`range(1, 6)` 生成 `1` 到 `5`。
- [ ] 忘记 `for` 循环语句中的 `in`。
  - 正确做法：基本格式为 `for 变量 in 可迭代对象:`。
- [ ] 直接修改 `for` 正在遍历的列表，导致遗漏元素或出现意外结果。
  - 正确做法：初学阶段先不要在遍历过程中修改原列表；需要修改时可遍历副本或创建新列表。

## 自测问题

1. `if` 语句后面需要什么类型的结果？
2. `if`、`elif`、`else` 末尾都需要写什么符号？
3. 为什么 Python 的代码块必须缩进？
4. `if score >= 60` 与 `if score = 60` 有什么区别？
5. 编写代码：输入一个整数，输出“正数”“负数”或“零”。
6. `case _` 的作用是什么？
7. 什么场景更适合使用 `match...case` 而不是 `if...elif...else`？
8. `while` 循环会在什么条件下继续执行？
9. 为什么计数循环中通常需要写 `count += 1`？
10. `break` 在 `while` 循环中有什么作用？
11. `range(1, 6)` 会产生哪些数字？
12. `for` 循环中的 `in` 有什么作用？
13. 什么情况下应优先选择 `for`，什么情况下选择 `while`？

## 关联内容

- 前置知识：变量、输入输出、比较运算符、逻辑运算符。
- 后续知识：`continue` 语句、循环嵌套、列表。
- 练习：输入成绩，按优秀、合格、不合格三个等级输出评价。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
