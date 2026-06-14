# Python 模块与包

> 模块和包是 Python 组织代码的方式。随着程序越来越复杂，把代码分散到不同的文件中，可以更好地管理和复用代码。

---

## 一、什么是模块？

### 模块的概念

> **模块（Module）**：一个 `.py` 文件就是一个模块。模块可以定义函数、类和变量，也可以包含可执行的代码。

### 模块的作用

> 可以把模块理解为一个「工具包」，每个工具包内部有各种不同的工具（函数、类等），可供我们使用。

### 模块的分类

| 类型           | 说明              | 示例                           |
| -------------- | ----------------- | ------------------------------ |
| **内置模块**   | Python 自带的模块 | `random`、`time`、`os`、`math` |
| **第三方模块** | 需要安装的模块    | `numpy`、`pandas`、`requests`  |
| **自定义模块** | 自己编写的模块    | 自己创建的 `.py` 文件          |

---

## 二、模块的导入方式

### 2.1 导入方式对比

| 导入方式             | 语法                              | 调用方式        | 适用场景               |
| -------------------- | --------------------------------- | --------------- | ---------------------- |
| 导入整个模块         | `import 模块名`                   | `模块名.功能()` | 使用模块中多个功能     |
| 导入并起别名         | `import 模块名 as 别名`           | `别名.功能()`   | 模块名较长时           |
| 导入指定功能         | `from 模块名 import 功能`         | `功能()`        | 只使用一两个功能       |
| 导入所有功能         | `from 模块名 import *`            | `功能()`        | 使用多个功能（不推荐） |
| 导入指定功能并起别名 | `from 模块名 import 功能 as 别名` | `别名()`        | 功能名冲突时           |

### 2.2 示例代码

```python
# 方式1：import 导入整个模块
import random
print(random.randint(1, 5))

# 方式2：from 导入指定功能
from random import randint
print(randint(1, 5))

# 方式3：from 导入所有功能（不推荐）
from random import *
print(randint(1, 5))
print(choice('abc'))      # 随机选择一个字符
print(sample('abc', 2))   # 随机选择2个字符

# 方式4：import 起别名
import random as r
print(r.randint(1, 5))

# 方式5：from 导入并起别名
from random import randint as rint
print(rint(1, 5))
```

### 2.3 导入方式对比图

```
┌─────────────────────────────────────────────────────────┐
│                    import 模块名                          │
│  把整个模块导入，需要通过「模块名.」来调用                    │
│  优点：不会污染命名空间                                     │
│  示例：import random → random.randint()                   │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│              from 模块名 import 功能名                     │
│  只导入指定的功能，可以直接使用功能名                         │
│  优点：调用时更简洁                                         │
│  缺点：可能与其他模块的同名功能冲突                           │
│  示例：from random import randint → randint()             │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                 from 模块名 import *                       │
│  导入所有功能，可以直接使用任何功能                           │
│  缺点：污染命名空间，不推荐使用                               │
│  示例：from random import * → randint()、choice()...      │
└─────────────────────────────────────────────────────────┘
```

---

## 三、常用内置模块

### 3.1 `random` 模块

```python
import random

# 随机整数 [a, b]
print(random.randint(1, 10))

# 随机浮点数 [0.0, 1.0)
print(random.random())

# 从序列中随机选择一个元素
print(random.choice(['苹果', '香蕉', '橙子']))

# 从序列中随机选择 n 个元素（不重复）
print(random.sample(['苹果', '香蕉', '橙子', '葡萄'], 2))

# 打乱列表顺序
lst = [1, 2, 3, 4, 5]
random.shuffle(lst)
print(lst)

# 生成随机浮点数 [a, b)
print(random.uniform(1.5, 10.5))
```

### 3.2 验证码生成案例

```python
import random
import string

# 使用 random 生成验证码
def generate_code1(length=4):
    """随机生成验证码（数字+字母+下划线）"""
    s = '1234567890_abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
    return ''.join(random.sample(s, length))

# 使用 string 模块优化
def generate_code2(length=4):
    """使用 string 模块生成验证码"""
    s = string.digits + string.ascii_letters + '_'
    return ''.join(random.sample(s, length))

# 使用循环生成
def generate_code3(length=4):
    s = string.digits + string.ascii_letters + '_'
    code = ''
    for i in range(length):
        code += random.choice(s)
    return code

print(generate_code1(6))
print(generate_code2(6))
print(generate_code3(6))
```

### 3.3 `string` 模块

```python
import string

# 常用字符串常量
print(string.ascii_letters)   # 所有大小写字母 abcdefghijklmnopqrstuvwxyzABCDEF...
print(string.ascii_lowercase) # 小写字母
print(string.ascii_uppercase) # 大写字母
print(string.digits)          # 数字 0123456789
print(string.punctuation)     # 标点符号 !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
print(string.hexdigits)       # 十六进制数字 0123456789abcdefABCDEF
```

### 3.4 `math` 模块

```python
import math

# 数学常量
print(math.pi)          # 圆周率 π ≈ 3.14159
print(math.e)           # 自然常数 e ≈ 2.71828

# 数学函数
print(math.sqrt(9))     # 平方根 → 3.0
print(math.pow(2, 3))   # 幂运算 → 8.0
print(math.log(100))    # 自然对数
print(math.log10(100))  # 以10为底的对数 → 2.0
print(math.factorial(5))# 阶乘 → 120

# 三角函数
print(math.sin(math.pi/2))  # 正弦 → 1.0
print(math.cos(0))          # 余弦 → 1.0
```

### 3.5 `datetime` 模块

```python
import datetime

# 获取当前日期时间
now = datetime.datetime.now()
print(now)                     # 2024-06-08 14:30:45.123456
print(now.year)                # 年份
print(now.month)               # 月份
print(now.day)                 # 日期
print(now.hour)                # 小时
print(now.minute)              # 分钟

# 格式化输出
print(now.strftime('%Y-%m-%d %H:%M:%S'))  # 2024-06-08 14:30:45

# 日期运算
today = datetime.date.today()
tomorrow = today + datetime.timedelta(days=1)
print(tomorrow)
```

---

## 四、自定义模块

### 4.1 创建自定义模块

创建一个 `my_m1.py` 文件：

```python
# my_m1.py

def add(a, b):
    """加法函数"""
    print('my_m1中add函数被调用了...')
    return a + b

def sub(a, b):
    """减法函数"""
    return a - b

# 测试代码（只在当前文件运行时执行）
if __name__ == '__main__':
    print('测试 add 函数：', add(1, 2))
    print('测试 sub 函数：', sub(5, 3))
```

### 4.2 导入自定义模块

```python
# 方式1：import 导入
import my_m1
print(my_m1.add(3, 4))   # 7

# 方式2：from 导入指定功能
from my_m1 import add
print(add(3, 4))         # 7

# 方式3：from 导入所有功能
from my_m1 import *
print(add(3, 4))         # 7
print(sub(5, 2))         # 3
```

### 4.3 `__name__` 变量

```python
# 每个模块都有一个 __name__ 变量
# 当模块被直接运行时，__name__ 的值为 '__main__'
# 当模块被导入时，__name__ 的值为模块名

# 常用写法：让测试代码只在直接运行时执行
if __name__ == '__main__':
    # 这里的代码只在直接运行当前文件时执行
    # 被其他文件导入时不会执行
    print("这是测试代码")
```

### 4.4 `__all__` 变量

```python
# my_m3.py

# __all__ 列表限制了 from 模块 import * 能导入的功能
__all__ = ['show1', 'show2', 'show3']

def show1():
    print("show1")

def show2():
    print("show2")

def show3():
    print("show3")

def show4():
    print("show4")  # 不在 __all__ 中，不会被导入
```

```python
# 导入测试
from my_m3 import *

show1()  # ✅ 可以调用
show2()  # ✅ 可以调用
show3()  # ✅ 可以调用
# show4()  # ❌ 报错！因为 __all__ 中没有指定
```

---

## 五、包（Package）

### 5.1 什么是包？

> **包（Package）**：本质是一个目录，用于组织多个模块。包目录中必须有一个 `__init__.py` 文件（可以是空文件），用于标识这是一个包。

### 5.2 包的目录结构

```
my_package/              # 包名
    ├── __init__.py      # 包标识文件（必须有）
    ├── my_m4.py         # 模块1
    ├── my_m5.py         # 模块2
    └── my_m6.py         # 模块3
```

### 5.3 导入包中的模块

```python
# 方式1：from 包名 import 模块名
from my_package import my_m4
my_m4.test()

# 方式2：import 包名.模块名
import my_package.my_m4
my_package.my_m4.test()

# 方式3：import 包名.模块名 as 别名
import my_package.my_m4 as m4
m4.test()

# 方式4：from 包名.模块名 import 功能
from my_package.my_m4 import test
test()
```

### 5.4 `__init__.py` 的作用

```python
# my_package/__init__.py

# 1. 标识这是一个包（必须存在）

# 2. 通过 __all__ 控制 from 包 import * 的导入内容
__all__ = ['my_m4', 'my_m5']

# 3. 可以在 __init__.py 中导入子模块，简化导入
from . import my_m4
from . import my_m5
```

```python
# 使用 from 包 import * 导入
from my_package import *

my_m4.test()  # ✅ 可以调用（__all__ 中指定了）
my_m5.test()  # ✅ 可以调用
# my_m6.test()  # ❌ 报错（__all__ 中没有指定）
```

---

## 六、模块搜索路径

当导入一个模块时，Python 会按以下顺序搜索：

```
1. 当前目录（执行脚本所在的目录）
2. PYTHONPATH 环境变量中的目录
3. Python 安装目录（标准库）
4. 第三方库安装目录（site-packages）
```

查看模块搜索路径：

```python
import sys
print(sys.path)
```

---

## 七、常见问题与解决方案

### 问题1：导入自定义模块时自动执行了测试代码

**原因**：模块中直接写了函数调用，没有使用 `if __name__ == '__main__':` 保护。

**解决**：将测试代码放在 `if __name__ == '__main__':` 中。

```python
# ❌ 错误写法
def add(a, b):
    return a + b
add(1, 2)  # 导入时会执行

# ✅ 正确写法
def add(a, b):
    return a + b
if __name__ == '__main__':
    add(1, 2)  # 只在直接运行时执行
```

### 问题2：多个模块导入同名功能冲突

**原因**：后导入的同名功能会覆盖先导入的。

**解决**：使用 `import 模块` 方式，或者使用别名。

```python
# ❌ 冲突
from my_m1 import add
from my_m2 import add  # 覆盖了上面的 add
add(3, 4)  # 调用的是 my_m2 的 add

# ✅ 解决方案1：使用别名
from my_m1 import add as add1
from my_m2 import add as add2
add1(3, 4)
add2(3, 4)

# ✅ 解决方案2：使用 import 方式
import my_m1
import my_m2
my_m1.add(3, 4)
my_m2.add(3, 4)
```

### 问题3：`from 包 import *` 不生效

**原因**：需要在包的 `__init__.py` 中定义 `__all__` 列表。

**解决**：在 `__init__.py` 中添加 `__all__ = ['模块名1', '模块名2']`

---

## 知识点总结

### 模块导入方式速查表

| 导入方式             | 语法                            | 调用方式      |
| -------------------- | ------------------------------- | ------------- |
| 导入整个模块         | `import 模块`                   | `模块.功能()` |
| 导入并起别名         | `import 模块 as 别名`           | `别名.功能()` |
| 导入指定功能         | `from 模块 import 功能`         | `功能()`      |
| 导入所有功能         | `from 模块 import *`            | `功能()`      |
| 导入指定功能并起别名 | `from 模块 import 功能 as 别名` | `别名()`      |

### 包与模块对比

| 概念 | 本质       | 标识          | 作用               |
| ---- | ---------- | ------------- | ------------------ |
| 模块 | `.py` 文件 | 文件名        | 组织函数、类、变量 |
| 包   | 目录       | `__init__.py` | 组织模块           |

### 特殊变量

| 变量       | 作用                                    |
| ---------- | --------------------------------------- |
| `__name__` | 当前模块名（直接运行时为 `'__main__'`） |
| `__all__`  | 限制 `from 模块 import *` 能导入的内容  |
| `__file__` | 当前文件的路径                          |

---

## 练习题

1. **随机密码**：使用 `random` 和 `string` 模块，生成一个包含大小写字母、数字、特殊符号的 8 位随机密码
2. **自定义模块**：创建一个 `math_utils.py` 模块，包含 `add`、`sub`、`mul`、`div` 四个函数，并测试
3. **包的使用**：创建一个 `shapes` 包，包含 `circle.py`、`rectangle.py` 两个模块，分别计算面积和周长
4. **模块搜索路径**：打印 `sys.path`，找出 Python 的模块搜索路径=
