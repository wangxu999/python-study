# Python 面向对象基础

## 一句话结论

面向对象是一种组织代码的方式：先用类（`class`）描述一类事物的共同特征和行为，再根据类创建具体的对象。类像图纸，对象像按照图纸创建出来的具体实例。

## 为什么学习面向对象

当程序中的数据和操作这些数据的函数关系紧密时，把它们放在同一个类中会更清楚。

例如，一个学生有姓名、年龄、成绩等数据，也有自我介绍、计算平均分等行为。使用类可以把这些内容组织在一起。

| 概念 | 可以理解为 | 示例 |
| --- | --- | --- |
| 类（class） | 图纸、模板、分类规则 | `Student` |
| 对象（object） | 按图纸创建出的具体事物 | `student_1` |
| 属性（attribute） | 对象保存的数据 | `name`、`age` |
| 方法（method） | 对象能执行的行为 | `introduce()`、`get_average()` |

## 定义类和创建对象

使用 `class` 关键字定义类。类名通常使用大驼峰命名法：每个单词首字母大写，例如 `Student`、`BankAccount`。

```python
class Student:
    pass


student_1 = Student()
print(student_1)
```

`Student` 是类，`student_1` 是通过 `Student()` 创建的对象，也称为类的实例。

## 属性

属性是对象保存的数据。可以在对象创建后，为对象添加属性。

```python
class Student:
    pass


student_1 = Student()
student_1.name = "小王"
student_1.age = 18

print(student_1.name)  # 小王
print(student_1.age)   # 18
```

不同对象可以拥有不同的属性值：

```python
student_2 = Student()
student_2.name = "小李"
student_2.age = 19

print(student_1.name)  # 小王
print(student_2.name)  # 小李
```

这种创建后再逐个赋值的方式可行，但当属性较多时容易遗漏。实际中更常使用构造方法 `__init__` 初始化属性。

## 方法和 `self`

定义在类中的函数称为方法。实例方法的第一个参数通常写为 `self`，它表示“当前正在调用该方法的对象”。

```python
class Student:
    def introduce(self) -> None:
        print("我是学生")


student_1 = Student()
student_1.introduce()  # 我是学生
```

调用 `student_1.introduce()` 时，Python 会自动把 `student_1` 传给 `self`。因此调用时不用手动写 `student_1.introduce(student_1)`。

通过 `self.属性名`，方法可以访问或修改当前对象的数据：

```python
class Student:
    def introduce(self) -> None:
        print(f"我叫{self.name}，今年{self.age}岁")


student_1 = Student()
student_1.name = "小王"
student_1.age = 18
student_1.introduce()  # 我叫小王，今年18岁
```

## 构造方法 `__init__`

`__init__` 是一种特殊方法。每次使用 `类名()` 创建对象时，Python 会自动调用它，常用于为对象设置初始属性。

```python
class Student:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age


student_1 = Student("小王", 18)
student_2 = Student("小李", 19)

print(student_1.name)  # 小王
print(student_2.age)   # 19
```

上例中：

- `name`、`age` 是创建对象时传入的参数。
- `self.name`、`self.age` 是保存到当前对象中的属性。
- `student_1` 和 `student_2` 各自保存独立的数据。

## 完整示例：学生成绩

```python
class Student:
    def __init__(self, name: str, scores: list[float]) -> None:
        self.name = name
        self.scores = scores

    def get_average(self) -> float:
        return sum(self.scores) / len(self.scores)

    def introduce(self) -> None:
        average = self.get_average()
        print(f"我是{self.name}，平均分是{average:.1f}")


student = Student("小王", [88, 95, 76])
student.introduce()  # 我是小王，平均分是86.3
```

在这个例子中，`name` 和 `scores` 是属性；`get_average()` 和 `introduce()` 是方法。`introduce()` 可以直接调用同一个对象的 `self.get_average()`。

> 注意：若成绩列表可能为空，需要先判断 `self.scores`，否则 `len(self.scores)` 为 `0` 时会出现除零错误。

## 修改对象属性

对象创建后，属性可以被读取和修改。

```python
class Account:
    def __init__(self, owner: str, balance: float) -> None:
        self.owner = owner
        self.balance = balance

    def deposit(self, money: float) -> None:
        self.balance += money


account = Account("小王", 100.0)
account.deposit(50.0)
print(account.balance)  # 150.0
```

这里的 `deposit()` 方法修改的是当前账户对象的 `balance` 属性。

## 类属性和实例属性

定义在 `__init__` 中、使用 `self.` 保存的属性称为实例属性，每个对象各自拥有一份。

```python
class Student:
    def __init__(self, name: str) -> None:
        self.name = name
```

直接定义在类中的属性称为类属性，通常由所有对象共享。

```python
class Student:
    school_name = "阳光学校"

    def __init__(self, name: str) -> None:
        self.name = name


student_1 = Student("小王")
student_2 = Student("小李")

print(student_1.school_name)  # 阳光学校
print(student_2.school_name)  # 阳光学校
```

### 实例属性与类属性对比

| 对比项 | 实例属性 | 类属性 |
| --- | --- | --- |
| 定义位置 | 通常在 `__init__` 中通过 `self.属性名` 定义 | 直接写在 `class` 的代码块中 |
| 数据归属 | 每个对象各自保存一份 | 类保存一份，多个对象默认共享 |
| 典型内容 | 姓名、年龄、账户余额 | 学校名称、圆周率、默认配置 |
| 推荐访问方式 | `student.name` | `Student.school_name` |
| 修改后影响 | 只影响当前对象 | 通过类修改时影响未被覆盖的所有对象 |

```python
class Student:
    school_name = "阳光学校"  # 类属性

    def __init__(self, name: str, age: int) -> None:
        self.name = name  # 实例属性
        self.age = age    # 实例属性


student_1 = Student("小王", 18)
student_2 = Student("小李", 19)

student_1.age = 20

print(student_1.age)  # 20
print(student_2.age)  # 19，未受影响
print(Student.school_name)  # 阳光学校
```

### 修改类属性

应该通过类名修改类属性。这样所有没有同名实例属性的对象都会读取到新值。

```python
class Student:
    school_name = "阳光学校"

    def __init__(self, name: str) -> None:
        self.name = name


student_1 = Student("小王")
student_2 = Student("小李")

Student.school_name = "希望学校"

print(student_1.school_name)  # 希望学校
print(student_2.school_name)  # 希望学校
```

如果通过对象赋值同名属性，通常不会修改类属性，而是给该对象新增一个同名实例属性，称为“遮蔽”类属性：

```python
student_1.school_name = "临时学校"

print(student_1.school_name)  # 临时学校
print(student_2.school_name)  # 希望学校
print(Student.school_name)    # 希望学校
```

所以，共享设置应使用 `Student.school_name = "新学校"` 修改；只想让某个对象不同，则使用 `student_1.school_name = "临时学校"`。

### 不要把每个对象的数据放进可变类属性

列表、字典、集合等可变对象若作为类属性，会被所有对象共享。下面的写法容易产生意外：

```python
class Student:
    courses: list[str] = []  # 不推荐：所有 Student 对象共用同一个列表


student_1 = Student()
student_2 = Student()
student_1.courses.append("Python")

print(student_2.courses)  # ['Python']，也被影响了
```

如果每位学生都应有自己的课程列表，应在 `__init__` 中创建实例属性：

```python
class Student:
    def __init__(self, name: str) -> None:
        self.name = name
        self.courses: list[str] = []  # 每个对象都有独立列表


student_1 = Student("小王")
student_2 = Student("小李")
student_1.courses.append("Python")

print(student_1.courses)  # ['Python']
print(student_2.courses)  # []
```

初学阶段可以优先掌握实例属性；当数据确实应该由全部对象共享时，再考虑类属性。

## 魔法方法

魔法方法也叫特殊方法，方法名两边都有两个下划线，例如 `__init__`、`__str__`。Python 会在特定操作发生时自动调用它们。

例如，创建对象时会自动调用 `__init__`；使用 `print(对象)` 时会尝试调用 `__str__`。通常不需要自己直接写 `对象.__str__()`，而是使用对应的普通语法。

| 魔法方法 | 触发方式 | 常见用途 |
| --- | --- | --- |
| `__init__` | `Student("小王", 18)` | 初始化对象属性 |
| `__str__` | `print(student)` | 定义给用户看的字符串内容 |
| `__repr__` | 在交互式环境直接输入 `student` | 定义给开发者查看的对象表示 |
| `__len__` | `len(obj)` | 定义对象的长度或数量 |
| `__eq__` | `obj1 == obj2` | 定义两个对象是否相等 |
| `__add__` | `obj1 + obj2` | 定义对象相加时的行为 |

### `__str__`：让 `print(对象)` 更易读

没有定义 `__str__` 时，直接打印对象通常会看到类似内存地址的内容，不容易阅读：

```text
<__main__.Student object at 0x000001...>
```

定义 `__str__` 后，可以返回更有意义的字符串。注意它必须返回 `str` 类型。

```python
class Student:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

    def __str__(self) -> str:
        return f"Student(name={self.name}, age={self.age})"


student = Student("小王", 18)
print(student)  # Student(name=小王, age=18)
```

### `__len__`：支持 `len(对象)`

`__len__` 应返回非负整数。下面让班级对象的长度等于学生数量：

```python
class Classroom:
    def __init__(self, students: list[str]) -> None:
        self.students = students

    def __len__(self) -> int:
        return len(self.students)


classroom = Classroom(["小王", "小李", "小张"])
print(len(classroom))  # 3
```

### `__eq__`：定义 `==` 的比较规则

默认情况下，两个不同创建的对象，即使属性值相同，使用 `==` 也通常会得到 `False`，因为默认比较的是不是同一个对象。

```python
class Student:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Student):
            return NotImplemented
        return self.name == other.name and self.age == other.age


student_1 = Student("小王", 18)
student_2 = Student("小王", 18)
print(student_1 == student_2)  # True
```

这里先用 `isinstance()` 判断类型，避免将 `Student` 与无关对象直接按学生属性比较。

### `__add__`：支持对象相加

`__add__` 可以定义 `+` 的行为。下面的例子让两个购物车相加，得到一个新购物车：

```python
class Cart:
    def __init__(self, count: int) -> None:
        self.count = count

    def __add__(self, other: object) -> "Cart":
        if not isinstance(other, Cart):
            return NotImplemented
        return Cart(self.count + other.count)


cart_1 = Cart(2)
cart_2 = Cart(3)
total_cart = cart_1 + cart_2
print(total_cart.count)  # 5
```

并非所有类都需要定义 `__add__`。只有对象之间的“相加”有清晰、自然的业务含义时才使用它。

### 使用原则

- 只实现符合对象语义的魔法方法，不要为了“看起来高级”而重载运算符。
- `__str__` 返回给用户阅读的信息，应简洁清晰。
- `__repr__` 更适合调试；许多情况下可以让它返回能清楚表示对象状态的字符串。
- 自定义 `__eq__` 后，如果对象还要作为字典键或集合元素，需要进一步理解 `__hash__` 的规则；初学阶段先不要随意实现 `__hash__`。

## 类与对象的命名规范

- 类名使用大驼峰命名法，例如 `StudentScore`、`BankAccount`。
- 方法名和属性名使用小写字母，多个单词用下划线连接，例如 `get_average`、`school_name`。
- 一个类应尽量只负责一类相关事情，不要把所有功能都塞进同一个类。

## 易错点

- [ ] 定义了类，却忘记使用 `类名()` 创建对象。
  - 正确做法：`Student` 是类，`Student()` 才会创建对象。
- [ ] 在实例方法中漏写第一个参数 `self`。
  - 正确做法：实例方法定义时，第一个参数通常写 `self`。
- [ ] 调用实例方法时手动传入 `self`。
  - 正确做法：写 `student.introduce()`；Python 会自动传入当前对象。
- [ ] 在方法里写 `name`，却想访问对象属性。
  - 正确做法：对象属性应写为 `self.name`。
- [ ] 把 `self.name = name` 写反成 `name = self.name`。
  - 正确做法：创建对象时，将参数 `name` 保存到属性 `self.name`。
- [ ] 以为不同对象修改属性会互相影响。
  - 正确做法：实例属性各自独立；类属性才是所有对象共享的。
- [ ] 使用 `student.school_name = "新学校"`，却想修改所有学生的学校名称。
  - 正确做法：这通常只会给当前对象创建同名实例属性；应写 `Student.school_name = "新学校"`。
- [ ] 将每个对象各自的数据放在列表类属性中。
  - 正确做法：可变的个人数据应在 `__init__` 中创建为实例属性，例如 `self.courses = []`。
- [ ] 直接对空成绩列表计算平均分。
  - 正确做法：计算前判断列表是否为空。
- [ ] 以为需要手动调用 `student.__str__()`。
  - 正确做法：定义好 `__str__` 后，直接使用 `print(student)`。
- [ ] `__str__` 中返回整数、列表等非字符串数据。
  - 正确做法：`__str__` 必须返回字符串。
- [ ] 在 `__eq__` 中不判断 `other` 的类型，直接访问 `other.name`。
  - 正确做法：先使用 `isinstance(other, Student)` 判断，并对无关类型返回 `NotImplemented`。
- [ ] 为没有合理含义的对象实现 `__add__`。
  - 正确做法：仅当 `+` 符合业务直觉时才实现该方法。

## 自测问题

1. 类和对象分别是什么？请用“图纸”和“具体物品”说明。
2. 使用什么关键字定义类？
3. `Student()` 的作用是什么？
4. `self` 代表什么？调用方法时需要手动传入吗？
5. `__init__` 在什么时候自动执行？
6. `self.name = name` 左右两侧分别表示什么？
7. 实例属性和类属性的主要区别是什么？
8. 请定义一个 `Book` 类，包含书名属性和 `show_info()` 方法。
9. 修改 `Student.school_name` 后，为什么多个学生对象通常都能读取到新值？
10. 为什么 `student_1.school_name = "临时学校"` 不会改变 `Student.school_name`？
11. 为什么不建议把每名学生的课程列表定义为类属性？
12. `print(student)` 时，Python 会优先调用哪个魔法方法？
13. 若希望 `len(classroom)` 返回班级人数，应定义哪个魔法方法？
14. `__eq__` 用来定义哪个运算符的规则？
15. 为什么不应该为所有类都定义 `__add__`？

## 关联内容

- 前置知识：变量、数据类型、数据容器、函数、类型注解、模块。
- 后续知识：封装、继承、多态、类方法、静态方法、`__hash__`。
- 练习：定义一个 `Rectangle` 类，创建时接收长和宽，编写 `get_area()` 方法返回面积。

## 复习状态

- 首次记录：2026-09-13
- 最近复习：
- 掌握程度：了解
