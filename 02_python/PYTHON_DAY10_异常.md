# Python 异常处理

> 程序运行时难免会出错，异常处理让程序在遇到错误时能够「优雅地处理」，而不是直接崩溃。

---

## 一、什么是异常？

### 异常的概念

> **异常**：程序在运行过程中发生的错误，如果不处理，程序会中断执行。

### 常见的异常类型

| 异常类型            | 说明         | 示例                            |
| ------------------- | ------------ | ------------------------------- |
| `NameError`         | 变量名未定义 | `print(a)` 但 `a` 没定义        |
| `FileNotFoundError` | 文件不存在   | `open('a.txt', 'r')` 文件不存在 |
| `ZeroDivisionError` | 除零错误     | `1 / 0`                         |
| `ValueError`        | 值错误       | `int('abc')`                    |
| `IndexError`        | 索引错误     | `[1,2,3][10]`                   |
| `KeyError`          | 键错误       | `{'a':1}['b']`                  |
| `TypeError`         | 类型错误     | `'1' + 1`                       |

### 系统默认处理方式

```python
print('程序开始~')

# NameError: 变量a没有定义
print(a)           # ❌ 程序在此中断

# 下面的代码不会执行
print('程序结束~')
```

**输出**：
```
程序开始~
Traceback (most recent call last):
  File "demo.py", line 4, in <module>
    print(a)
NameError: name 'a' is not defined
```

> ⚠️ 系统默认处理方式：在异常位置**直接中断程序**，后面的代码无法执行。

---

## 二、`try-except` 异常处理

### 基本语法

```python
try:
    可能会出现异常的代码
except:
    出现异常时执行的代码
```

### 示例

```python
print('程序开始~')

try:
    print(a)           # 这行会报错
    print(1 / 0)       # 这行不会执行（因为上面已经报错了）
except:
    print('报错了吧，下回注意~，接着去工作吧')

print('程序结束~')
```

**输出**：
```
程序开始~
报错了吧，下回注意~，接着去工作吧
程序结束~
```

> ✅ 使用 `try-except` 后，程序不会崩溃，异常被捕获后继续执行。

---

## 三、捕获具体异常

### 语法

```python
try:
    可能会出现异常的代码
except 异常类型 as 变量名:
    出现该异常时执行的代码
```

### 示例

```python
try:
    print(a)
except NameError as e:
    print(f"捕获到变量未定义的异常：{e}")
```

### 捕获多种异常

```python
# 方式1：多个 except 块
try:
    # 可能产生多种异常的代码
    num = int(input("请输入数字："))
    result = 10 / num
    print(result)
except ValueError:
    print("输入的不是有效数字！")
except ZeroDivisionError:
    print("除数不能为0！")

# 方式2：使用 Exception 捕获所有异常（推荐）
try:
    print(a)
    print(1 / 0)
except Exception as e:
    print(f"发生了异常：{e}")
```

### 异常类型继承关系

```
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception  ← 大部分异常的父类
    ├── NameError
    ├── ZeroDivisionError
    ├── ValueError
    ├── FileNotFoundError
    └── ...
```

> 💡 使用 `Exception` 可以捕获几乎所有常见的异常。

---

## 四、`try-except-else-finally`

### 完整语法

```python
try:
    可能会出现异常的代码
except Exception as e:
    出现异常时执行的代码
else:
    没有异常时执行的代码
finally:
    不管有没有异常都会执行的代码
```

### 示例

```python
print('程序开始~')

try:
    # 修改这里的代码测试不同情况
    print(a)  # 有异常
    # print("正常执行")  # 无异常
except Exception as e:
    print('except中是有异常执行的代码')
else:
    print('else中是没有异常执行的代码')
finally:
    print('finally中是不管有没有异常都执行的代码')

print('程序结束~')
```

### 各子句说明

| 子句      | 执行时机     | 使用场景                           |
| --------- | ------------ | ---------------------------------- |
| `try`     | 必定执行     | 放置可能出错的代码                 |
| `except`  | 有异常时执行 | 处理错误、记录日志                 |
| `else`    | 无异常时执行 | 没有错误时继续执行的代码           |
| `finally` | 必定执行     | 释放资源（关闭文件、数据库连接等） |

---

## 五、异常的传递性

> 异常可以在函数调用链中**向上传递**，直到被 `try-except` 捕获。

### 示例

```python
def show1():
    print('--------show1开始---------')
    print(1 / 0)  # 异常在这里产生
    print('--------show1结束---------')

def show2():
    print('--------show2开始---------')
    show1()  # 异常会向上传递到这里
    print('--------show2结束---------')

if __name__ == '__main__':
    print('程序开始~')
    show2()
    print('程序结束~')
```

**输出**：
```
程序开始~
--------show2开始---------
--------show1开始---------
Traceback (most recent call last):
  ...
ZeroDivisionError: division by zero
```

### 异常传递路线图

```
show1() 产生异常
    ↓ 向上传递
show2() 调用 show1()
    ↓ 向上传递
main 调用 show2()
    ↓ 向上传递
程序崩溃（如果没有捕获）
```

### 最佳实践：在根源处处理异常

```python
def show1():
    print('--------show1开始---------')
    try:
        print(1 / 0)  # 在产生异常的地方处理
    except Exception as e:
        print(f"捕获到异常：{e}")
    print('--------show1结束---------')

def show2():
    print('--------show2开始---------')
    show1()
    print('--------show2结束---------')

if __name__ == '__main__':
    print('程序开始~')
    show2()
    print('程序结束~')
```

**输出**：
```
程序开始~
--------show2开始---------
--------show1开始---------
捕获到异常：division by zero
--------show1结束---------
--------show2结束---------
程序结束~
```

> 💡 **原则**：在**异常产生的根源处**处理异常，可以避免异常向上传递导致程序崩溃。

---

## 六、主动抛出异常：`raise`

### 语法

```python
raise 异常类型("异常信息")
```

### 示例

```python
def set_age(age):
    if age < 0 or age > 150:
        raise ValueError(f"年龄 {age} 无效，应该在 0-150 之间")
    print(f"年龄设置为：{age}")

try:
    set_age(200)
except ValueError as e:
    print(f"错误：{e}")
```

**输出**：
```
错误：年龄 200 无效，应该在 0-150 之间
```

### 使用场景

- 函数参数校验
- 业务规则校验
- 自定义错误提示

---

## 七、自定义异常

### 语法

```python
class 自定义异常类(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)
```

### 示例

```python
# 定义自定义异常
class AgeTooSmallError(Exception):
    """年龄过小异常"""
    pass

class AgeTooLargeError(Exception):
    """年龄过大异常"""
    pass

def set_age(age):
    if age < 0:
        raise AgeTooSmallError(f"年龄 {age} 不能为负数")
    if age > 150:
        raise AgeTooLargeError(f"年龄 {age} 超过150岁")
    print(f"年龄设置为：{age}")

# 使用自定义异常
try:
    set_age(-5)
except AgeTooSmallError as e:
    print(f"年龄过小：{e}")
except AgeTooLargeError as e:
    print(f"年龄过大：{e}")
```

**输出**：
```
年龄过小：年龄 -5 不能为负数
```

---

## 八、常见异常速查表

| 异常                | 触发条件       | 示例                     |
| ------------------- | -------------- | ------------------------ |
| `NameError`         | 变量未定义     | `print(x)` 但 `x` 没定义 |
| `TypeError`         | 类型不匹配     | `'1' + 1`                |
| `ValueError`        | 值不正确       | `int('abc')`             |
| `ZeroDivisionError` | 除零           | `1 / 0`                  |
| `IndexError`        | 索引超出范围   | `[1,2,3][10]`            |
| `KeyError`          | 键不存在       | `{'a':1}['b']`           |
| `FileNotFoundError` | 文件不存在     | `open('no.txt')`         |
| `AttributeError`    | 属性不存在     | `None.append(1)`         |
| `ImportError`       | 导入模块不存在 | `import xxx`             |
| `KeyboardInterrupt` | 用户按 Ctrl+C  | -                        |

---

## 知识点总结

### 异常处理结构

```python
try:
    # 可能出错的代码
    pass
except 异常类型 as 变量:
    # 处理指定异常
    pass
except Exception as e:
    # 处理所有异常（兜底）
    pass
else:
    # 没有异常时执行
    pass
finally:
    # 无论是否有异常都执行（释放资源）
    pass
```

### 最佳实践

| 原则         | 说明                                         |
| ------------ | -------------------------------------------- |
| **精确捕获** | 尽量捕获具体的异常类型，而不是所有异常       |
| **根源处理** | 在异常产生的地方处理，避免向上传递           |
| **日志记录** | 生产环境应该记录异常日志，便于排查           |
| **合理使用** | 不要滥用异常，能用条件判断避免的就用条件判断 |

### 示例：文件操作中的异常处理

```python
def read_file(filename):
    """安全读取文件内容"""
    try:
        with open(filename, 'r', encoding='utf-8') as f:
            return f.read()
    except FileNotFoundError:
        print(f"文件 {filename} 不存在")
        return None
    except PermissionError:
        print(f"没有权限读取文件 {filename}")
        return None
    except Exception as e:
        print(f"读取文件时发生未知错误：{e}")
        return None

# 使用
content = read_file('a.txt')
if content:
    print(f"文件内容：{content}")
```

---

## 练习题

1. **除零处理**：写一个程序，接收用户输入的两个数字，输出除法结果，处理除零异常
2. **类型转换**：写一个程序，尝试将用户输入转换为整数，如果失败则提示重新输入
3. **文件读取**：写一个函数，安全读取文件，如果文件不存在则返回空字符串
4. **自定义异常**：定义一个 `PasswordError` 异常，在密码长度小于 6 位时抛出
