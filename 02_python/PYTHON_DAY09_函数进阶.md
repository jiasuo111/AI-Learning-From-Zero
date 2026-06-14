# Python 函数进阶

> 在掌握了函数基础之后，这一节我们来学习函数的更多高级特性：变量的作用域、多种参数类型、匿名函数、递归函数等。

---

## 一、函数基础回顾

### 函数的核心知识点

#### 定义函数

```python
def 函数名(形参):
    函数体
    return 返回值
```

#### 调用函数

```python
变量 = 函数名(实参)  # 变量接收返回值，如果没有 return 则收到 None
```

### 函数注意事项

| 规则             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| 先定义，再调用   | 函数必须定义后才能使用                                   |
| 不调用不执行     | 定义只是准备，调用才执行                                 |
| 调用一次执行一次 | 可以多次调用同一个函数                                   |
| 形参与实参对应   | 定义时有形参，调用时必须传入对应数量的实参               |
| 返回值建议接收   | 有返回值时，建议先用变量接收，而不是直接 `print(函数())` |

### 函数的基本形态

```python
# 1. 无参无返回值
def qiu_he():
    a, b = 10, 20
    print(a + b)

qiu_he()

# 2. 有参无返回值
def qiu_he(a, b):
    print(a + b)

qiu_he(10, 20)

# 3. 有参有返回值（最常用）
def qiu_he(a, b):
    return a + b

result = qiu_he(10, 20)
print(result)
```

### 函数说明文档

```python
def qiu_he(a, b):
    """
    功能：求两个数的和
    :param a: 整数
    :param b: 整数
    :return: 整数
    """
    return a + b
```

### 函数嵌套调用

```python
def test1():
    print('test1 开始')
    print('省略代码...')
    print('test1 结束')

def test2():
    print('test2 开始')
    test1()      # 在 test2 中调用 test1
    print('test2 结束')

if __name__ == '__main__':
    print('main 开始')
    test2()
    print('main 结束')
```

**输出顺序**：
```
main 开始
test2 开始
test1 开始
省略代码...
test1 结束
test2 结束
main 结束
```

---

## 二、全局变量与局部变量

### 概念区分

| 变量类型     | 定义位置 | 作用范围         |
| ------------ | -------- | ---------------- |
| **局部变量** | 函数内部 | 只能在函数内使用 |
| **全局变量** | 函数外部 | 函数内外都能使用 |

### 基本使用

```python
# 全局变量
num = 100

def test1():
    # 局部变量
    a = 10
    print(a)      # ✅ 可以访问
    print(num)    # ✅ 可以访问全局变量

def test2():
    print(num)    # ✅ 可以访问全局变量
    # print(a)    # ❌ 报错！a 是 test1 的局部变量

if __name__ == '__main__':
    test1()
    test2()
    print(num)    # ✅ 可以访问全局变量
```

### 局部变量与全局变量同名

当局部变量和全局变量同名时，函数内会**优先使用局部变量**（就近原则）。

```python
num = 100  # 全局变量

def test():
    num = 10  # 局部变量（同名，但不会修改全局变量）
    print(num)  # 输出：10

test()
print(num)  # 输出：100（全局变量未被修改）
```

### 在函数内修改全局变量

> ⚠️ **注意**：默认情况下，在函数内给同名变量赋值会**创建局部变量**，而不是修改全局变量。要修改全局变量，必须使用 `global` 关键字声明。

```python
num = 100  # 全局变量

def test1():
    num = 10      # 这是创建了局部变量，不是修改全局变量
    print(num)    # 10

def test2():
    global num    # 声明要修改的是全局变量
    num = 200     # 现在修改的是全局变量
    print(num)    # 200

test1()
print(num)  # 100（未被修改）

test2()
print(num)  # 200（已被修改）
```

---

## 三、多返回值

### 语法

```python
return 值1, 值2, 值3, ...
```

> `return` 多个值时，会自动**打包成元组**返回。

### 示例

```python
def show():
    return 1, 2, 3, 4, 5  # 多个值自动打包成元组

# 方式1：用一个变量接收（得到元组）
result = show()
print(result, type(result))  # (1, 2, 3, 4, 5) <class 'tuple'>

# 方式2：拆包接收
a, b, c, d, e = show()
print(a, b, c, d, e)  # 1 2 3 4 5
```

---

## 四、多种参数类型

### 1. 位置参数

> 实参和形参的**个数及顺序**必须一致。

```python
def show(name, age, height):
    print(f"{age}岁的{name}，身高{height}")

show("张三", 18, 178.8)  # 个数和顺序必须一致
```

### 2. 关键字参数

> 顺序可以不一致，但**个数必须一致**。使用 `形参名=实参值` 的形式。

```python
def show(name, age, height):
    print(f"{age}岁的{name}，身高{height}")

# 关键字传参：顺序无所谓
show(height=178.8, name="张三", age=18)

# 混合使用：位置参数必须在关键字参数之前
show("张三", height=178.8, age=18)  # ✅ 正确
# show(height=178.8, "张三", 18)   # ❌ 报错！位置参数必须在前面
```

### 3. 缺省参数（默认值）

> 定义函数时给形参设置默认值，调用时**可以省略**该参数。

```python
def show(name, age=18, height=188.8):
    print(f"{age}岁的{name}，身高{height}")

show("张三")                 # 使用默认值 age=18, height=188.8
show("张三", 20)             # 使用默认值 height=188.8
show("张三", 20, 175.5)      # 全部指定
```

> ⚠️ **注意**：缺省参数必须放在**位置参数之后**。

### 4. 可变参数

#### `*args`：接收位置参数

> 将传入的位置参数**打包成元组**。

```python
def show(*args):
    print(args, type(args))

show()           # () <class 'tuple'>
show(1)          # (1,) <class 'tuple'>
show(1, 2, 3)    # (1, 2, 3) <class 'tuple'>
```

#### `**kwargs`：接收关键字参数

> 将传入的关键字参数**打包成字典**。

```python
def show(**kwargs):
    print(kwargs, type(kwargs))

show()                          # {} <class 'dict'>
show(a=1)                       # {'a': 1} <class 'dict'>
show(a=1, b=2, c=3)             # {'a': 1, 'b': 2, 'c': 3} <class 'dict'>
```

#### 综合使用

```python
def get_sum(*args, **kwargs):
    total = 0
    # 累加位置参数
    for i in args:
        total += i
    # 累加关键字参数的值
    for i in kwargs.values():
        total += i
    return total

result = get_sum(1, 2, 3, 4, a=5, b=6)
print(result)  # 21
```

---

## 五、匿名函数（lambda 表达式）

### 语法

```python
lambda 形参1, 形参2, ... : 单行表达式
```

### 特点

| 特点     | 说明                                       |
| -------- | ------------------------------------------ |
| 简洁     | 适用于简单函数                             |
| 匿名     | 不需要 `def` 定义函数名                    |
| 隐式返回 | 表达式的结果自动作为返回值                 |
| 限制     | 只能写**单行表达式**，不能写循环、多行语句 |

### 示例

```python
# 传统函数
def get_sum(a, b):
    return a + b

# 匿名函数（直接调用）
print((lambda a, b: a + b)(1, 2))   # 3

# 匿名函数（赋值给变量）
my_sum = lambda a, b: a + b
print(my_sum(1, 2))  # 3
```

### lambda 作为参数传递

```python
# 定义函数，接收一个函数作为参数
def show(func, a, b):
    result = func(a, b)
    print(result)

# 传递普通函数
def get_sum(a, b):
    return a + b
show(get_sum, 10, 2)   # 12

# 传递 lambda 表达式（更简洁）
show(lambda a, b: a + b, 10, 2)   # 12
show(lambda a, b: a - b, 10, 2)   # 8
show(lambda a, b: a * b, 10, 2)   # 20
show(lambda a, b: a / b, 10, 2)   # 5.0
```

---

## 六、递归函数

### 概念

> **递归**：函数直接或间接调用自身。把一个大问题分解成多个性质相同但规模更小的子问题。

### 递归三要素

| 要素         | 说明                               |
| ------------ | ---------------------------------- |
| **明确功能** | 确定函数要做什么（参数和返回值）   |
| **结束条件** | 找到递归结束的条件（防止无限循环） |
| **等价关系** | 找到缩小问题规模的规律             |

### 示例1：递归求 1 到 n 的和

```python
def get_sum(x):
    # 1. 明确功能：求 1 到 x 的和
    # 2. 结束条件：当 x == 1 时，返回 1
    if x == 1:
        return 1
    # 3. 等价关系：1 到 x 的和 = x + 1 到 (x-1) 的和
    return x + get_sum(x - 1)

print(get_sum(5))  # 15
```

**执行过程**：
```
get_sum(5) = 5 + get_sum(4)
           = 5 + (4 + get_sum(3))
           = 5 + (4 + (3 + get_sum(2)))
           = 5 + (4 + (3 + (2 + get_sum(1))))
           = 5 + (4 + (3 + (2 + 1)))
           = 5 + (4 + (3 + 3))
           = 5 + (4 + 6)
           = 5 + 10
           = 15
```

### 示例2：递归求阶乘

```python
def get_f(x):
    # 功能：求 x 的阶乘
    # 结束条件：1! = 1, 2! = 2
    if x in (1, 2):
        return x
    # 等价关系：n! = n * (n-1)!
    return x * get_f(x - 1)

print(get_f(5))  # 120
```

### 示例3：斐波那契数列（兔子数列）

> **问题**：一对小兔，一个月长成大兔，再过一个月生一对小兔...假设没有死亡，求第 n 个月的兔子对数。

**规律**：`f(1) = 1, f(2) = 1, f(3) = 2, f(4) = 3, f(5) = 5...`
即：`f(n) = f(n-1) + f(n-2)`

```python
def get_rabbits(month):
    # 功能：返回第 month 个月的兔子对数
    # 结束条件：前两个月都是 1 对
    if month in (1, 2):
        return 1
    # 等价关系：本月 = 上月 + 上上月
    return get_rabbits(month - 1) + get_rabbits(month - 2)

print(get_rabbits(12))  # 144（一年后的兔子对数）
```

### 递归的注意事项

| 优点                  | 缺点                 |
| --------------------- | -------------------- |
| 代码简洁，逻辑清晰    | 效率较低，可能栈溢出 |
| 适合解决树形/分治问题 | 深度过大时性能差     |

> 💡 **提示**：递归适合解决「问题可以分解为相同子问题」的场景，但深度较大时建议使用循环代替。

---

## 知识点总结

### 函数参数类型对比

| 参数类型   | 格式              | 调用时          | 使用场景           |
| ---------- | ----------------- | --------------- | ------------------ |
| 位置参数   | `def f(a, b)`     | `f(1, 2)`       | 最常用             |
| 关键字参数 | `def f(a, b)`     | `f(b=2, a=1)`   | 参数多时提高可读性 |
| 缺省参数   | `def f(a=1, b=2)` | `f()` 或 `f(3)` | 提供默认值         |
| 可变参数   | `def f(*args)`    | `f(1,2,3)`      | 不确定参数个数     |
| 可变关键字 | `def f(**kwargs)` | `f(a=1,b=2)`    | 接收键值对参数     |

### 变量作用域

```python
# 全局变量
global_var = 100

def func():
    # 局部变量
    local_var = 10
    global global_var  # 声明要修改全局变量
    global_var = 200
```

### 递归模板

```python
def recursion(参数):
    # 1. 结束条件
    if 满足结束条件:
        return 基础值
    # 2. 递归调用（缩小问题规模）
    return recursion(缩小后的参数)
```

---

## 练习题

1. **变量作用域**：写一个程序，演示全局变量和局部变量的区别
2. **多返回值**：写一个函数，返回一个数字的平方和立方
3. **可变参数**：写一个函数，计算传入的所有数字的平均值
4. **匿名函数**：用 `lambda` 实现一个判断奇偶的函数
5. **递归**：用递归求 1 到 100 的和

---

> 