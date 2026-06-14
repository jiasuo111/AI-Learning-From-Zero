# Python 装饰器

> 装饰器是 Python 中一个非常强大且优雅的特性。它可以在**不修改原有函数代码**的情况下，为函数添加额外的功能，是代码复用和横切关注点（如日志、计时、权限校验）的经典解决方案。

------

## 一、装饰器入门

### 1.1 什么是装饰器？

**装饰器**：在不改变现有函数源代码以及函数调用方式的前提下，给函数增加额外功能的一种技术。

> 💡 **本质**：装饰器就是一个**闭包函数**。

| 装饰器核心 | 说明                             |
| :--------- | :------------------------------- |
| 有嵌套     | 函数内部定义函数                 |
| 有引用     | 内部函数引用外部函数的参数（fn） |
| 有返回     | 返回内部函数                     |

### 1.2 装饰器的雏形

**需求**：为评论功能、下载功能添加登录验证，但不修改原有函数的代码和调用方式。

python

```
def check(fn):
    """登录验证装饰器"""
    def inner():
        print('验证登录')   # 添加的额外功能
        fn()                # 调用原函数
    return inner

# 评论功能（需要登录）
def comment():
    print('发表评论')

# 使用装饰器增强（手动方式）
comment = check(comment)
comment()

# 下载功能（需要登录）
def download():
    print('下载文件')

download = check(download)
download()
```



**输出**：

text

```
验证登录
发表评论
验证登录
下载文件
```



### 1.3 装饰器语法糖：`@`

Python 提供了 `@` 语法糖来简化装饰器的使用。

python

```
def check(fn):
    def inner():
        print('验证登录')
        fn()
    return inner

@check  # 等价于 comment = check(comment)
def comment():
    print('发表评论')

@check
def download():
    print('下载文件')

comment()   # 直接调用即可
download()
```



### 1.4 装饰器的作用：计算程序执行时间

python

```
import time

def get_time(fn):
    """计算函数执行时间的装饰器"""
    def inner():
        begin = time.time()
        fn()
        end = time.time()
        print(f'函数执行时间：{end - begin:.4f} 秒')
    return inner

@get_time
def demo():
    # 模拟耗时操作
    total = 0
    for i in range(1000000):
        total += i
    print(f'计算结果：{total}')

demo()
```



**输出**：

text

```
计算结果：499999500000
函数执行时间：0.0523 秒
```



------

## 二、装饰器进阶

### 2.1 带参数的装饰器

原函数可能有参数，装饰器需要能够处理这些参数。

python

```
def logging(fn):
    def inner(*args, **kwargs):
        print('日志信息：正在努力计算')
        fn(*args, **kwargs)  # 传递参数
    return inner

@logging
def sum_num(*args, **kwargs):
    result = 0
    for i in args:
        result += i
    for i in kwargs.values():
        result += i
    print(f'计算结果：{result}')

sum_num(10, 20, a=30, b=40)
```



**输出**：

text

```
日志信息：正在努力计算
计算结果：100
```



### 2.2 带返回值的装饰器

原函数如果有返回值，装饰器也需要返回该值。

python

```
def logging(fn):
    def inner(*args, **kwargs):
        print('日志信息：正在努力计算')
        return fn(*args, **kwargs)  # 返回原函数的结果
    return inner

@logging
def sub_num(a, b):
    return a - b

result = sub_num(20, 10)
print(f'计算结果：{result}')
```



**输出**：

text

```
日志信息：正在努力计算
计算结果：10
```



### 2.3 通用版本装饰器

结合不定长参数和返回值，可以写出能装饰任何函数的通用装饰器。

python

```
def logging(fn):
    """通用装饰器：支持任何参数和返回值"""
    def inner(*args, **kwargs):
        print('日志信息：正在努力计算')
        return fn(*args, **kwargs)
    return inner

@logging
def add(a, b):
    return a + b

@logging
def multiply(a, b, c):
    return a * b * c

print(add(10, 20))        # 日志信息... 30
print(multiply(2, 3, 4))  # 日志信息... 24
```



### 2.4 带参数的装饰器（装饰器工厂）

有时我们需要给装饰器本身传递参数，比如根据不同条件执行不同的操作。

python

```
def logging(flag):
    """装饰器工厂：根据 flag 参数返回不同的装饰器"""
    def decorator(fn):
        def inner(*args, **kwargs):
            if flag == '+':
                print('日志信息：正在努力进行加法运算')
            elif flag == '-':
                print('日志信息：正在努力进行减法运算')
            else:
                print('日志信息：正在努力进行计算')
            return fn(*args, **kwargs)
        return inner
    return decorator

@logging('+')  # 相当于 logging('+')(sum_num)
def sum_num(a, b):
    return a + b

@logging('-')
def sub_num(a, b):
    return a - b

print(sum_num(10, 20))  # 日志信息：正在努力进行加法运算 → 30
print(sub_num(100, 80)) # 日志信息：正在努力进行减法运算 → 20
```



**理解带参数的装饰器**：

text

```
@logging('+')
def sum_num(a, b):
    return a + b

# 等价于
sum_num = logging('+')(sum_num)
```



------

## 三、装饰器执行流程详解

### 3.1 装饰器的加载时机

装饰器在**函数定义时**就会执行，而不是在函数调用时。

python

```
def decorator(fn):
    print('装饰器正在加载...')
    def inner():
        print('装饰器功能执行')
        return fn()
    return inner

@decorator
def my_func():
    print('原函数执行')

print('--- 分隔线 ---')
my_func()
```



**输出**：

text

```
装饰器正在加载...
--- 分隔线 ---
装饰器功能执行
原函数执行
```



### 3.2 多个装饰器的执行顺序

多个装饰器时，从下往上包裹，从上往下执行。

python

```
def decorator_a(fn):
    def inner():
        print('装饰器 A 开始')
        fn()
        print('装饰器 A 结束')
    return inner

def decorator_b(fn):
    def inner():
        print('装饰器 B 开始')
        fn()
        print('装饰器 B 结束')
    return inner

@decorator_a
@decorator_b
def my_func():
    print('原函数执行')

my_func()
```



**输出**：

text

```
装饰器 A 开始
装饰器 B 开始
原函数执行
装饰器 B 结束
装饰器 A 结束
```



**理解**：

- `@decorator_a @decorator_b` 等价于 `decorator_a(decorator_b(my_func))`
- 执行顺序：外层装饰器 A 先开始 → 内层装饰器 B → 原函数 → 内层结束 → 外层结束

------

## 四、装饰器实际应用场景

| 应用场景     | 说明                             |
| :----------- | :------------------------------- |
| **日志记录** | 记录函数的调用信息、参数、返回值 |
| **权限校验** | 检查用户是否有权限执行某个函数   |
| **性能计时** | 计算函数执行时间                 |
| **缓存**     | 缓存函数计算结果，提高性能       |
| **事务管理** | 数据库操作的事务控制             |
| **路由注册** | Web 框架中的 URL 路由映射        |

### 案例：权限校验装饰器

python

```
def require_login(fn):
    def inner(*args, **kwargs):
        # 模拟检查用户是否已登录
        is_login = check_login_status()
        if not is_login:
            print('请先登录！')
            return None
        return fn(*args, **kwargs)
    return inner

def require_permission(permission):
    def decorator(fn):
        def inner(*args, **kwargs):
            if not has_permission(permission):
                print(f'没有权限：{permission}')
                return None
            return fn(*args, **kwargs)
        return inner
    return decorator

# 使用示例
@require_login
def comment():
    print('发表评论')

@require_permission('admin')
def delete_user(user_id):
    print(f'删除用户：{user_id}')
```



------

## 五、常用装饰器（Python 内置）

### 5.1 `@staticmethod` 和 `@classmethod`

python

```
class MyClass:
    @staticmethod
    def static_method():
        """静态方法：不需要 self 或 cls"""
        print('静态方法')
    
    @classmethod
    def class_method(cls):
        """类方法：第一个参数是 cls"""
        print('类方法')
```



### 5.2 `@property`

python

```
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def area(self):
        """把方法变成属性调用"""
        return 3.14159 * self._radius ** 2

c = Circle(5)
print(c.area)  # 像属性一样调用，不需要括号
```



### 5.3 `@functools.wraps`

使用装饰器时，原函数的元信息（如 `__name__`、`__doc__`）会丢失，`@wraps` 可以解决这个问题。

python

```
from functools import wraps

def logging(fn):
    @wraps(fn)  # 保留原函数的元信息
    def inner(*args, **kwargs):
        print('日志信息')
        return fn(*args, **kwargs)
    return inner

@logging
def add(a, b):
    """加法函数"""
    return a + b

print(add.__name__)  # 输出：add（不加 @wraps 会输出 inner）
print(add.__doc__)   # 输出：加法函数
```



------

## 知识点总结

### 装饰器核心模板

python

```
# 1. 基本装饰器
def decorator(fn):
    def inner(*args, **kwargs):
        # 添加额外功能
        result = fn(*args, **kwargs)
        # 可以处理结果
        return result
    return inner

# 2. 带参数的装饰器
def decorator_with_param(param):
    def decorator(fn):
        def inner(*args, **kwargs):
            # 可以使用 param
            return fn(*args, **kwargs)
        return inner
    return decorator
```



### 装饰器快速参考

| 装饰器类型   | 模板                                                         |
| :----------- | :----------------------------------------------------------- |
| 无参无返回值 | `def decorator(fn): def inner(): fn(); return inner`         |
| 带参数       | `def decorator(fn): def inner(*args, **kwargs): fn(*args, **kwargs); return inner` |
| 带返回值     | `def decorator(fn): def inner(*args, **kwargs): return fn(*args, **kwargs); return inner` |
| 装饰器带参数 | `def outer(param): def decorator(fn): def inner(): ...; return inner; return decorator` |

### 装饰器执行顺序

text

```
函数定义时：
    装饰器代码执行（装饰过程）
    
函数调用时：
    外层装饰器开始 → 内层装饰器开始 → 原函数 → 内层装饰器结束 → 外层装饰器结束
```



------

## 练习题

1. **计时装饰器**：编写一个装饰器 `@timer`，输出函数的执行时间
2. **重试装饰器**：编写一个装饰器 `@retry(max_attempts=3)`，当函数执行失败时自动重试
3. **缓存装饰器**：编写一个装饰器 `@cache`，缓存函数的计算结果，相同参数直接返回缓存
4. **权限装饰器**：编写一个装饰器 `@require_role('admin')`，检查调用者是否有管理员权限