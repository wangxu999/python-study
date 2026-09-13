# Python 异常

## 一句话结论

异常（Exception）是程序运行时发生的问题，例如除以零、输入内容无法转换为数字、文件不存在。使用 `try` / `except` 可以捕获并处理预期内的异常，避免程序直接中断。

## 错误与异常

初学阶段可以先区分两类问题：

| 类型 | 发生时间 | 示例 | 能否用 `try` / `except` 处理 |
| --- | --- | --- | --- |
| 语法错误（SyntaxError） | 程序开始运行前 | 忘写冒号、括号不匹配 | 不能，必须先修改代码 |
| 异常（Exception） | 程序运行时 | 除零、类型不匹配、文件不存在 | 可以处理预期内的情况 |

例如，下面代码语法正确，但运行时会出现 `ZeroDivisionError`：

```python
result = 10 / 0
```

## 为什么要处理异常

异常处理不是为了隐藏所有错误，而是为了对用户可能出现的正常错误输入、文件缺失、网络失败等情况给出合理提示或补救。

例如，用户输入年龄时可能输入文字；程序应提醒重新输入，而不是直接崩溃。

## 基本语法：`try` 和 `except`

将可能出错的代码放进 `try` 块；发生指定异常时，Python 会跳到对应的 `except` 块。

```python
try:
    可能出错的代码
except 异常类型:
    出错后的处理代码
```

示例：

```python
try:
    age = int(input("请输入年龄："))
    print(f"明年你 {age + 1} 岁")
except ValueError:
    print("请输入整数年龄")
```

当用户输入 `18` 时，会正常输出结果；输入 `十八` 时，`int()` 会抛出 `ValueError`，程序会输出提示而不是中断。

## 捕获多个异常

不同错误可以分别处理，给出更准确的提示：

```python
try:
    number = int(input("请输入一个整数："))
    result = 100 / number
    print(result)
except ValueError:
    print("输入内容不是整数")
except ZeroDivisionError:
    print("除数不能为 0")
```

多个异常也可以放进同一个元组中处理：

```python
try:
    number = int(input("请输入一个整数："))
    print(100 / number)
except (ValueError, ZeroDivisionError):
    print("请输入非零整数")
```

## 获取异常信息

使用 `as` 可以把异常对象保存到变量中，便于记录或显示具体错误信息。

```python
try:
    result = 10 / 0
except ZeroDivisionError as error:
    print(f"计算失败：{error}")
```

输出：

```text
计算失败：division by zero
```

给用户看的提示通常应简洁；详细的 `error` 信息更适合开发调试或写入日志。

## `else` 和 `finally`

- `else`：只有 `try` 中没有发生异常时才执行。
- `finally`：无论是否发生异常，通常都会执行，适合放清理操作。

```python
try:
    number = int(input("请输入一个整数："))
except ValueError:
    print("输入格式错误")
else:
    print(f"输入的数字是：{number}")
finally:
    print("本次输入处理结束")
```

执行顺序：

```text
try 成功：try -> else -> finally
try 出错：try -> except -> finally
```

`finally` 常用于关闭文件、数据库连接等资源。即使处理中途出错，也应尽量完成清理工作。

## 捕获所有常规异常

当确实需要兜底处理时，可以捕获 `Exception`：

```python
try:
    # 某段可能出现多种异常的代码
    pass
except Exception as error:
    print(f"程序发生错误：{error}")
```

不要使用空的 `except:`。它会连 `KeyboardInterrupt` 等不应轻易忽略的情况也捕获，并且会掩盖真正的程序错误。

在业务代码中，应优先捕获具体异常，例如 `ValueError`、`FileNotFoundError`，而不是一开始就写 `except Exception`。

## 主动抛出异常：`raise`

当数据不符合规则时，可以使用 `raise` 主动抛出异常，让调用者知道这个操作不能继续。

```python
def withdraw(balance: float, money: float) -> float:
    if money <= 0:
        raise ValueError("取款金额必须大于 0")
    if money > balance:
        raise ValueError("余额不足")
    return balance - money


try:
    print(withdraw(100.0, 120.0))
except ValueError as error:
    print(f"操作失败：{error}")
```

输出：

```text
操作失败：余额不足
```

`raise` 不是“程序一定崩溃”。如果上层有对应的 `try` / `except`，异常可以被接住并处理。

## 常见异常类型

| 异常类型 | 常见原因 | 示例 |
| --- | --- | --- |
| `ValueError` | 值的格式或范围不符合要求 | `int("abc")` |
| `TypeError` | 对不支持该操作的类型进行运算 | `"1" + 1` |
| `KeyError` | 字典中不存在指定键 | `student["score"]` |
| `IndexError` | 列表或元组下标越界 | `names[10]` |
| `ZeroDivisionError` | 除数为零 | `10 / 0` |
| `FileNotFoundError` | 要打开的文件不存在 | `open("missing.txt")` |
| `AttributeError` | 对象没有指定属性或方法 | `name.append("x")` |

## 完整示例：安全读取成绩

```python
def read_score() -> float | None:
    try:
        score = float(input("请输入 0 到 100 的成绩："))
        if not 0 <= score <= 100:
            raise ValueError("成绩必须在 0 到 100 之间")
    except ValueError as error:
        print(f"输入无效：{error}")
        return None
    else:
        return score
    finally:
        print("成绩输入结束")


score = read_score()
if score is not None:
    print(f"记录的成绩是：{score}")
```

## 易错点

- [ ] 将语法错误当作异常处理。
  - 正确做法：语法错误必须先改正确，`try` / `except` 无法解决。
- [ ] 用空的 `except:` 忽略所有错误。
  - 正确做法：优先捕获具体异常；需要兜底时使用 `except Exception as error:` 并记录错误信息。
- [ ] 把大量正常业务判断都写进异常处理。
  - 正确做法：例如判断余额是否充足，应优先使用 `if`；只有确实需要中断并通知调用者时再 `raise`。
- [ ] 以为 `else` 在异常发生后执行。
  - 正确做法：`else` 只在 `try` 中没有异常时执行。
- [ ] 以为 `finally` 只在报错时执行。
  - 正确做法：通常无论是否出错，`finally` 都会执行。
- [ ] 捕获异常后什么也不做。
  - 正确做法：至少给出提示、记录日志、返回合理结果或重新抛出异常。
- [ ] 只写 `raise ValueError`，却没有说明错误原因。
  - 正确做法：提供清楚的信息，例如 `raise ValueError("年龄不能小于 0")`。

## 自测问题

1. 语法错误和运行时异常有什么区别？
2. `try` 块中发生 `ValueError` 时，程序会执行哪个代码块？
3. `except ValueError as error` 中的 `error` 是什么？
4. `else` 和 `finally` 分别在什么情况下执行？
5. 为什么不推荐使用空的 `except:`？
6. 为什么应该优先捕获具体异常，而不是直接捕获 `Exception`？
7. `raise ValueError("余额不足")` 的作用是什么？
8. 编写一个函数：输入除数，发生输入错误或除数为零时给出提示。

## 关联内容

- 前置知识：输入输出、流程控制、函数、类型注解、模块。
- 后续知识：文件读写、日志、第三方库请求、调试。
- 练习：编写一个函数，接收字典和键；键不存在时捕获 `KeyError` 并返回 `None`。

## 复习状态

- 首次记录：2026-09-13
- 最近复习：
- 掌握程度：了解
