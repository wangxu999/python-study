# Python 输入与输出

## 一句话结论

`input()` 用于获取键盘输入的数据，`print()` 用于将数据输出到控制台。它们是 Python 程序与用户进行简单交互的基础。

## input 函数

`input()` 的功能是获取用户在键盘输入的数据。

基本语法：

```python
变量名 = input(提示信息)
```

示例：

```python
name = input("请输入你的姓名：")
print(f"你好，{name}！")
```

运行时，控制台会先显示提示信息，等待用户输入内容并按 Enter 键；输入的内容会保存到变量 `name` 中。

### input 的返回类型

无论用户输入数字、文字还是其他内容，`input()` 得到的默认都是字符串 `str`。

```python
age = input("请输入年龄：")

print(type(age))
```

如果输入 `18`，输出仍然是：

```text
<class 'str'>
```

需要进行数值计算时，应先转换类型：

```python
age = int(input("请输入年龄："))
print(f"明年你将是 {age + 1} 岁。")
```

## print 函数

`print()` 的功能是将数据输出到控制台。

基本语法：

```python
print(数据)
```

示例：

```python
print("你好，Python")
print(18)
print(True)
```

输出：

```text
你好，Python
18
True
```

### 输出变量

`print()` 可以直接输出变量的值。

```python
name = "小王"
score = 95

print(name)
print(score)
```

### 输出多个数据

`print()` 可以用英文逗号分隔多个数据，默认会在它们之间加一个空格。

```python
name = "小王"
age = 18

print("姓名：", name, "年龄：", age)
```

输出：

```text
姓名： 小王 年龄： 18
```

也可以使用 f-string 组织更自然的输出文本：

```python
print(f"姓名：{name}，年龄：{age}")
```

## 完整示例

```python
name = input("请输入姓名：")
age = int(input("请输入年龄："))

print(f"你好，{name}！")
print(f"明年你将是 {age + 1} 岁。")
```

## 易错点

- [ ] 认为 `input()` 输入的 `18` 是整数。
  - 正确做法：`input()` 默认返回 `str`；需要计算时使用 `int()` 或 `float()` 转换。
- [ ] 忘记在 `input()` 输入完成后按 Enter 键。
  - 正确做法：输入数据后按 Enter，程序才会继续执行。
- [ ] 在 `print()` 中输出文本时忘记加引号。
  - 错误示例：`print(你好)`。
  - 正确做法：`print("你好")`。
- [ ] 将 `input` 或 `print` 写成 `Input`、`Print`。
  - 正确做法：Python 区分大小写，函数名必须使用小写。

## 自测问题

1. `input()` 的功能是什么？
2. `input()` 获取 `18` 后，默认是什么类型？
3. 如何将用户输入的年龄转换为整数？
4. `print()` 的功能是什么？
5. 如何一次输出姓名和年龄？

## 关联内容

- 前置知识：变量、字符串、数据类型、f-string。
- 后续知识：类型转换、运算符、条件判断。
- 练习：编写程序，分别输入姓名和年龄，并输出一句完整的自我介绍。

## 复习状态

- 首次记录：2026-09-12
- 最近复习：
- 掌握程度：了解
