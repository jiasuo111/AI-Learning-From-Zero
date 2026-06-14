# Python 变量作用域与闭包

> 理解变量的作用域是编写高质量 Python 代码的基础。闭包则是函数式编程中的重要概念，它让我们可以在全局作用域中间接访问局部变量。

------

## 一、全局变量和局部变量

### 1.1 什么是作用域？

在 Python 代码中，**作用域**指的是变量生效的范围。主要分为两种：

| 作用域类型     | 说明                 |
| :------------- | :------------------- |
| **全局作用域** | 在整个代码文件中生效 |
| **局部作用域** | 只在函数内部生效     |

### 1.2 变量的分类

python

```
# 全局变量：在函数外部定义的变量
global_var = 100

def func():
    # 局部变量：在函数内部定义的变量
    local_var = 200
    print(local_var)  # 可以在函数内访问

print(global_var)     # 可以在函数外访问
func()
```



### 1.3 变量的访问范围

#### ① 局部作用域中可以访问局部变量

python

```
def func():
    num = 20  # 局部变量
    print(num)  # ✅ 可以访问

func()
```



#### ② 局部作用域中可以访问全局变量

python

```
num = 10  # 全局变量

def func():
    print(num)  # ✅ 可以访问

func()  # 10
```



#### ③ 全局作用域中不能访问局部变量

python

```
def func():
    num = 20  # 局部变量

func()
print(num)  # ❌ 报错！NameError: name 'num' is not defined
```



### 1.4 为什么全局作用域无法访问局部变量？

**原因**：Python 底层有一个**垃圾回收机制（Garbage Collection）**，主要作用是回收不再使用的内存空间，提高计算机运行效率。

当函数执行完毕后，函数内部的局部变量会被自动回收，以释放内存。因此，函数外部无法访问已经销毁的局部变量。

text

```
┌─────────────────────────────────────────────────────┐
│                    程序运行                          │
│  ┌─────────────────────────────────────────────┐    │
│  │  调用 func()                                 │    │
│  │      ↓                                       │    │
│  │  创建局部变量 num = 20  (占用内存)            │    │
│  │      ↓                                       │    │
│  │  func() 执行完毕                             │    │
│  │      ↓                                       │    │
│  │  垃圾回收 → 销毁 num，释放内存                │    │
│  └─────────────────────────────────────────────┘    │
│                                                      │
│  全局作用域无法访问 num（已被销毁）                   │
└─────────────────────────────────────────────────────┘
```



------

## 二、闭包（Closure）

### 2.1 什么是闭包？

> **闭包**：在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个使用外部函数变量的内部函数称为闭包。

### 2.2 闭包的构成条件（三步走）

| 步骤   | 条件       | 说明                         |
| :----- | :--------- | :--------------------------- |
| 第一步 | **有嵌套** | 一个函数内部定义了另一个函数 |
| 第二步 | **有引用** | 内部函数使用了外部函数的变量 |
| 第三步 | **有返回** | 外部函数返回了内部函数       |

python

```
def outer():
    num = 20        # 外部函数的局部变量
    def inner():    # ① 有嵌套
        print(num)  # ② 有引用（使用了外部函数的变量）
    return inner    # ③ 有返回（返回内部函数）

# 调用
f = outer()   # f 指向 inner 函数
f()           # 执行 inner，输出 20
```



### 2.3 闭包的作用

**核心作用**：在全局作用域中，实现**间接对局部变量进行访问**。

python

```
def outer():
    message = "Hello from closure!"
    def inner():
        print(message)
    return inner

# 即使 outer 函数已经执行完毕，闭包仍然保留了 message 变量
func = outer()
func()  # Hello from closure!
```



> 💡 **理解**：正常情况下，`outer()` 执行完毕后，局部变量 `message` 会被销毁。但闭包「捕获」了这个变量，使其得以保留。这就是闭包的核心能力——**让函数「记住」它被创建时的环境**。

### 2.4 闭包的注意事项

| 优点                         | 缺点                                           |
| :--------------------------- | :--------------------------------------------- |
| 可以在全局作用域访问局部变量 | 外部函数的变量没有及时释放，会**消耗更多内存** |
| 实现数据封装和隐藏           | 过度使用可能导致内存泄漏                       |

### 2.5 在闭包内部修改外部变量

#### ❌ 错误写法（直接赋值无效）

python

```
def outer():
    num = 10
    def inner():
        num = 20  # 这不是修改，而是在 inner 内部创建了一个新的局部变量
    print('outer中的num：', num)  # 10
    inner()
    print('outer中的num：', num)  # 仍然是 10
    return inner

f = outer()
```



#### ✅ 正确写法（使用 `nonlocal` 关键字）

python

```
def outer():
    num = 10
    def inner():
        nonlocal num  # 声明 num 不是局部变量，而是来自外层函数
        num = 20      # 现在修改的是 outer 中的 num
    print('outer中的num：', num)  # 10
    inner()
    print('outer中的num：', num)  # 20
    return inner

f = outer()
```



### 2.6 `nonlocal` vs `global`

| 关键字     | 作用范围             | 使用场景                               |
| :--------- | :------------------- | :------------------------------------- |
| `global`   | 全局作用域           | 在函数内部修改**全局变量**             |
| `nonlocal` | 嵌套函数的外层作用域 | 在内部函数中修改**外层函数的局部变量** |

python

```
# global 示例
count = 0

def increment():
    global count
    count += 1

# nonlocal 示例
def outer():
    count = 0
    def inner():
        nonlocal count
        count += 1
    return inner
```



------

## 三、闭包综合案例

### 案例：累加器

**需求**：创建一个函数，每次调用时传入一个数字，累加并输出总和。

python

```
def make_accumulator():
    """
    创建一个累加器
    闭包三步走：① 有嵌套 ② 有引用 ③ 有返回
    """
    total = 0  # 外部函数的局部变量
    
    def accumulator(num):
        nonlocal total  # 声明要修改外层变量
        total += num
        print(f"当前总和：{total}")
    
    return accumulator

# 创建累加器
acc = make_accumulator()

acc(1)  # 当前总和：1
acc(2)  # 当前总和：3
acc(3)  # 当前总和：6
acc(4)  # 当前总和：10
```



**执行过程分析**：

text

```
执行 acc = make_accumulator()
    ↓
total = 0，定义 accumulator，返回 accumulator
    ↓
acc(1) → accumulator(1) → total += 1 → total = 1
    ↓
acc(2) → accumulator(2) → total += 2 → total = 3（闭包保留了 total 的值）
    ↓
acc(3) → accumulator(3) → total += 3 → total = 6
    ↓
acc(4) → accumulator(4) → total += 4 → total = 10
```



> 💡 **关键理解**：由于闭包的特性，`total` 这个局部变量在 `make_accumulator` 执行完毕后并没有被销毁，而是被内部的 `accumulator` 函数一直引用着，所以能够持续累加。

------

## 四、实际应用场景

闭包在 Python 中有很多实际应用场景：

| 应用场景     | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| **装饰器**   | 这是闭包最经典的应用，用于在不修改原函数代码的情况下增强功能 |
| **数据封装** | 创建私有变量，对外只暴露操作接口                             |
| **回调函数** | 在事件驱动编程中保存状态                                     |
| **惰性求值** | 延迟计算，只在需要时才执行                                   |

### 简单示例：数据封装

python

```
def create_counter():
    """创建一个计数器，外部无法直接修改计数值"""
    count = 0
    
    def get_count():
        return count
    
    def increment():
        nonlocal count
        count += 1
    
    # 返回一个包含多个函数的字典
    return {"get": get_count, "inc": increment}

counter = create_counter()
print(counter["get"]())  # 0
counter["inc"]()
counter["inc"]()
print(counter["get"]())  # 2
# 无法直接修改 count 变量，实现了封装
```



------

## 知识点总结

### 变量作用域对比

| 变量类型     | 定义位置 | 访问范围 | 生命周期     |
| :----------- | :------- | :------- | :----------- |
| **全局变量** | 函数外部 | 整个文件 | 程序运行期间 |
| **局部变量** | 函数内部 | 函数内部 | 函数执行期间 |

### 闭包核心要素

python

```
def outer():
    # 外部函数的局部变量
    captured_var = 0
    
    def inner():
        nonlocal captured_var  # 如需修改则声明
        # 使用外部变量
        return captured_var
    
    return inner
```



### 闭包 vs 普通函数

| 对比项   | 普通函数         | 闭包                       |
| :------- | :--------------- | :------------------------- |
| 变量来源 | 参数、全局变量   | 可以捕获外层函数的局部变量 |
| 状态保持 | 需要全局变量或类 | 自带状态，无需全局变量     |
| 内存占用 | 较小             | 相对较大（保留外部变量）   |

------

## 练习题

1. **作用域理解**：分析以下代码的输出结果

   python

   ```
   x = 10
   def func():
       x = 20
       print(x)
   func()
   print(x)
   ```

   

2. **简单闭包**：编写一个闭包，实现两个数的加法，第一次调用传入第一个数，第二次调用传入第二个数并返回和

3. **计数器**：用闭包实现一个计数器，每次调用返回当前计数，同时支持重置功能

4. **nonlocal 练习**：解释以下代码的输出结果

   python

   ```
   def outer():
       a = 1
       def inner():
           nonlocal a
           a += 1
           return a
       return inner
   
   f1 = outer()
   f2 = outer()
   print(f1())
   print(f1())
   print(f2())
   print(f2())
   ```

   

![02闭包](E:\黑马学习资料\各种文件\面试\01_python进阶\00总结图片\02闭包.png)