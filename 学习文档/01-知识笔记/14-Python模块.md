# Python 模块

## 一句话结论

模块就是一个 Python 文件（通常是 `.py` 文件），里面可以放变量、函数和类。通过 `import` 导入模块，可以复用已有代码，避免在每个文件中重复编写相同功能。

## 为什么使用模块

- 将不同功能拆分到不同文件，让代码更清晰。
- 复用自己以前写过的函数和变量。
- 使用 Python 自带的标准库，例如 `math`、`random`、`datetime`。
- 使用第三方库，例如 `requests`、`pandas`。

例如，`math` 是 Python 自带的数学模块，`math.py` 就是一个模块文件。

## 导入模块

### 导入整个模块

使用 `import 模块名` 导入模块。使用模块中的内容时，要写 `模块名.成员名`。

```python
import math

result = math.sqrt(16)
print(result)  # 4.0
```

这种写法来源清晰，看到 `math.sqrt()` 就知道 `sqrt` 来自 `math` 模块。

### 给模块起别名

模块名较长，或想避免和当前变量重名时，可以使用 `as` 起别名。

```python
import math as m

print(m.pi)  # 3.141592653589793
```

别名只在当前文件中有效。导入后应使用别名 `m`，不能再写 `math.pi`。

### 导入模块中的指定内容

使用 `from 模块名 import 成员名`，可以直接导入模块里的指定函数、变量或类。

```python
from math import sqrt, pi

print(sqrt(25))  # 5.0
print(pi)
```

也可以为指定内容起别名：

```python
from math import sqrt as square_root

print(square_root(9))  # 3.0
```

### 导入全部内容

```python
from math import *
```

这会把模块内可导入的许多名字直接放到当前文件中。初学时要知道这种写法，但实际项目中通常不推荐使用，因为容易不知道一个函数来自哪里，也可能发生名称冲突。

### 导入方式对比

| 写法 | 使用模块内容的方式 | 优点 | 注意点 / 适用情况 |
| --- | --- | --- | --- |
| `import math` | `math.sqrt(16)` | 来源清楚，不容易重名 | 推荐的通用写法 |
| `import math as m` | `m.sqrt(16)` | 模块名较长时更简洁 | 别名应清晰易懂；导入后只能使用 `m` |
| `from math import sqrt` | `sqrt(16)` | 只使用少量成员时写法简洁 | 要明确知道 `sqrt` 来自哪个模块 |
| `from math import sqrt as square_root` | `square_root(16)` | 可避免成员重名，也可让名称更好理解 | 不要起含义模糊的别名 |
| `from math import *` | `sqrt(16)` | 临时演示时少写代码 | 不推荐用于正式项目，容易造成名称冲突 |
| `from tools.calculator import add` | `add(3, 5)` | 可直接导入包中需要的成员 | 适用于项目中的包和模块 |

一般情况下优先使用 `import 模块名`；只需要模块中少量、明确的功能时，再使用 `from 模块名 import 成员名`。

## 常用标准库模块

标准库随 Python 一起安装，不需要额外安装。

| 模块 | 常见用途 | 示例 |
| --- | --- | --- |
| `math` | 数学计算 | `math.sqrt(16)` |
| `random` | 随机数、随机选择 | `random.randint(1, 10)` |
| `datetime` | 日期和时间 | `datetime.datetime.now()` |
| `os` | 与操作系统、文件路径交互 | `os.getcwd()` |
| `json` | JSON 数据转换 | `json.dumps(data)` |

示例：随机抽取一名学生。

```python
import random

students = ["小王", "小李", "小张"]
winner = random.choice(students)
print(winner)
```

## 导入自己编写的模块

假设同一个文件夹中有两个文件：

```text
项目文件夹/
├── calculator.py
└── main.py
```

`calculator.py`：

```python
def add(a: float, b: float) -> float:
    return a + b


def subtract(a: float, b: float) -> float:
    return a - b
```

`main.py`：

```python
import calculator

print(calculator.add(3, 5))       # 8
print(calculator.subtract(10, 4))  # 6
```

运行 `main.py` 时，Python 会找到同一文件夹内的 `calculator.py` 并导入它。

如果只需一个函数，也可以这样写：

```python
from calculator import add

print(add(3, 5))  # 8
```

## 模块被导入时会执行代码

首次导入模块时，模块顶层（没有缩进在函数或类里的）代码会执行一次。

`message.py`：

```python
print("message 模块已加载")


def show_message() -> None:
    print("你好")
```

`main.py`：

```python
import message

message.show_message()
```

输出：

```text
message 模块已加载
你好
```

因此，模块顶层一般只放定义；不希望导入时执行的测试代码，应放进 `if __name__ == "__main__":` 中。

## `__name__ == "__main__"`

每个 Python 文件都有一个内置变量 `__name__`。

- 直接运行一个文件时，它的 `__name__` 是 `"__main__"`。
- 这个文件被其他文件导入时，它的 `__name__` 通常是模块名。

```python
def add(a: int, b: int) -> int:
    return a + b


if __name__ == "__main__":
    # 只有直接运行当前文件时，才执行这里的测试代码。
    print(add(3, 5))
```

这样既可以单独运行当前文件测试，又不会在其他文件导入它时自动输出测试结果。

## 包的基本概念

当模块越来越多时，可以用文件夹对模块分类。这样的模块文件夹通常称为包（package）。

```text
项目文件夹/
├── main.py
└── tools/
    ├── __init__.py
    ├── calculator.py
    └── text_tools.py
```

`__init__.py` 是包的初始化文件。现代 Python 中没有它的文件夹也可能被识别为包，但初学阶段和许多项目中仍常保留该文件。

在 `main.py` 中可以导入包里的模块：

```python
from tools.calculator import add

print(add(3, 5))
```

## 第三方模块

第三方模块不是 Python 自带的，通常需要先通过 `pip` 安装，再在代码中导入。

```powershell
py -m pip install requests
```

安装后才能使用：

```python
import requests
```

在 PyCharm 中，要确认安装第三方模块时使用的 Python 解释器与运行代码时使用的解释器相同，否则可能出现“已安装但无法导入”的情况。

## 易错点

- [ ] 将文件命名为 `math.py`、`random.py`、`json.py` 等标准库模块名。
  - 正确做法：避免与标准库或第三方模块同名，否则 Python 可能导入当前文件而不是目标模块。
- [ ] `import math` 后直接写 `sqrt(16)`。
  - 正确做法：应写 `math.sqrt(16)`；除非使用了 `from math import sqrt`。
- [ ] 导入后仍使用原模块名，而不是别名。
  - 正确做法：`import math as m` 后应写 `m.sqrt(16)`。
- [ ] 使用 `from 模块 import *` 后出现同名函数却不知道来自哪里。
  - 正确做法：优先导入整个模块或明确导入需要的成员。
- [ ] 将测试用的 `print()` 直接写在模块顶层。
  - 正确做法：测试代码放在 `if __name__ == "__main__":` 内。
- [ ] 安装了第三方库，PyCharm 仍提示找不到模块。
  - 正确做法：检查 PyCharm 项目解释器和安装时 `py -m pip` 所用的解释器是否一致。

## 自测问题

1. 什么是 Python 模块？
2. `import math` 与 `from math import sqrt` 有什么区别？
3. `import random as r` 后，如何调用随机整数函数？
4. 为什么不推荐经常使用 `from 模块名 import *`？
5. 模块被第一次导入时，哪些代码会执行？
6. `if __name__ == "__main__":` 通常用来解决什么问题？
7. 包和模块有什么区别？
8. 第三方模块使用前通常需要做什么？

## 关联内容

- 前置知识：函数、类型注解、文件与文件夹的基本概念。
- 后续知识：`pip`、虚拟环境、常用标准库、异常处理、项目目录结构。
- 练习：创建 `converter.py`，写一个将摄氏温度转换为华氏温度的函数；再创建 `main.py` 导入并调用它。

## 复习状态

- 首次记录：2026-09-13
- 最近复习：
- 掌握程度：了解
