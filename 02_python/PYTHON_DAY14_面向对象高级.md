# Python 面向对象编程（进阶）

> 继承是面向对象编程的三大特性之一，它让代码能够更好地复用，体现类与类之间的共性与个性关系。

------

## 一、Python 中的继承

### 1.1 什么是继承？

类是用来描述现实世界中同一组事物的共有特性的抽象模型。但是类也有上下级和范围之分，比如：

> 生物 → 动物 → 哺乳动物 → 灵长类动物 → 人类 → 黄种人

从哲学上说，这就是**共性与个性**之间的关系。在 OOP 代码中，我们需要通过**类的继承**来体现这种关系。

**简单理解**：如果一个类 A 使用了另一个类 B 的成员（属性和方法），我们就说 A 类继承了 B 类。这也体现了 OOP 中**代码重用的特性**。

### 1.2 继承的基本语法

python

```
class 父类(object):
    pass

class 子类(父类):
    pass

# 子类自动拥有父类中的所有公共属性和方法
```



**案例**：Person 类与 Teacher、Student 类之间的继承关系

python

```
class Person(object):
    def eat(self):
        print('I can eat food!')
    
    def speak(self):
        print('I can speak!')

class Teacher(Person):
    pass

class Student(Person):
    pass

teacher = Teacher()
teacher.eat()    # I can eat food!
teacher.speak()  # I can speak!

student = Student()
student.eat()    # I can eat food!
student.speak()  # I can speak!
```



### 1.3 与继承相关的几个概念

| 概念               | 说明                                   | 方向        |
| :----------------- | :------------------------------------- | :---------- |
| **继承**           | 一个类从另一个已有的类获得其成员的特性 | 子类 → 父类 |
| **派生**           | 从一个已有的类产生一个新的类           | 父类 → 子类 |
| **父类（基类）**   | 被继承的类                             | -           |
| **子类（派生类）** | 继承后的类                             | -           |
| **扩展**           | 在子类中增加自己特有的特性             | -           |

| 继承类型   | 说明                                              |
| :--------- | :------------------------------------------------ |
| **单继承** | 一个类只能继承自一个父类（大多数 OOP 语言的特性） |
| **多继承** | 一个类同时继承多个父类（C++、Python 等支持）      |

------

### 1.4 单继承

**基本语法**：

python

```
# 1. 定义一个共性类（父类）
class Person(object):
    pass

# 2. 定义一个个性类（子类）
class Student(Person):
    pass
```



**案例**：汽车可以分为汽油车和电动车

python

```
class Car(object):
    def run(self):
        print('I can run')

class GasolineCar(Car):
    pass

class ElectricCar(Car):
    pass

bwm = GasolineCar()
bwm.run()  # I can run
```



### 1.5 单继承的传递性（多层继承）

如果一个类 A 继承了 B，B 又继承了 C，则 A 会自动继承 C 中的所有公共属性和方法。

python

```
class C(object):
    def func(self):
        print('我是 C 类中的方法')

class B(C):
    pass

class A(B):
    pass

a = A()
a.func()  # 我是 C 类中的方法
```



**继承链**：A → B → C → object

### 1.6 编写面向对象代码中的常见问题

| 问题     | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| 类名问题 | 类名应使用大驼峰命名法，如 `ElectricCar`、`StudentInfo`      |
| 继承问题 | 建议让所有类都继承 `object`（Python 3 中可省略，但习惯保留） |
| 拼写问题 | `__init__` 容易写错为 `__int__`，注意区分                    |

------

### 1.7 多继承

**什么是多继承？**

Python 是少数支持多继承的语言之一。多继承允许一个类同时继承自多个父类。

**基本语法**：

python

```
class 子类(父类A, 父类B):
    pass
```



**案例**：汽油车 + 电动车 = 混合动力汽车

python

```
class GasolineCar(object):
    def run_with_gasoline(self):
        print('I can run with gasoline')

class ElectricCar(object):
    def run_with_electric(self):
        print('I can run with electric')

class HybridCar(GasolineCar, ElectricCar):
    pass

tesla = HybridCar()
tesla.run_with_gasoline()   # I can run with gasoline
tesla.run_with_electric()   # I can run with electric
```



> ⚠️ **注意**：实际开发中应尽量避免使用多继承，因为如果两个父类出现相同的方法名，会产生命名冲突。

------

### 1.8 子类重写父类属性和方法

**什么是重写？**

重写也叫做**覆盖**。当子类成员与父类成员名字相同时，子类成员会**覆盖**从父类继承下来的成员。

python

```
class Animal(object):
    def eat(self):
        print('吃')
    
    def call(self):
        print('叫')

class Dog(Animal):
    # 重写父类的 call 方法
    def call(self):
        print('汪汪叫')

class Cat(Animal):
    # 重写父类的 call 方法
    def call(self):
        print('喵喵叫')

wangcai = Dog()
wangcai.eat()   # 吃（继承自父类）
wangcai.call()  # 汪汪叫（重写后的方法）

miaomiao = Cat()
miaomiao.call() # 喵喵叫（重写后的方法）
```



**方法调用顺序**：

1. 先在子类中查找
2. 如果找不到，再向父类中查找
3. 以此类推，直到 object 类

------

### 1.9 `super()` 调用父类属性和方法

**作用**：在子类中调用父类的方法或属性。

**语法**（Python 3）：

python

```
super().方法名()
super().__init__(参数)
```



**案例**：汽车、汽油车、电动车

python

```
class Car(object):
    def __init__(self, brand, model, color):
        self.brand = brand
        self.model = model
        self.color = color
    
    def run(self):
        print('I can run')

class GasolineCar(Car):
    def run(self):
        print('I can run with gasoline')

class ElectricCar(Car):
    def __init__(self, brand, model, color, battery):
        # 调用父类的初始化方法
        super().__init__(brand, model, color)
        self.battery = battery
    
    def run(self):
        print(f'I can run with electric, battery: {self.battery} kWh')

bwm = GasolineCar('宝马', 'X5', '白色')
bwm.run()   # I can run with gasoline

tesla = ElectricCar('特斯拉', 'Model S', '红色', 70)
tesla.run() # I can run with electric, battery: 70 kWh
```



### 1.10 MRO（方法解析顺序）

MRO（Method Resolution Order）决定了 Python 在继承链中查找方法的顺序。

python

```
print(ElectricCar.__mro__)
# (<class '__main__.ElectricCar'>, <class '__main__.Car'>, <class 'object'>)
```



**查找规则**：

- 首先在自身类中查找
- 如果找不到，按 MRO 顺序向上查找
- 直到 object 类

------

## 二、Python 中的多态

### 2.1 什么是多态？

> **多态**：同一类事物有多种形态。不同对象使用相同方法，可以产生不同的执行结果。

**核心**：

- 子类重写父类方法
- 调用不同子类对象的相同父类方法，产生不同的结果

**多态的好处**：调用灵活，更容易编写通用代码，适应需求变化。

### 2.2 多态代码实现

python

```
class Fruit(object):
    def make_juice(self):
        print('I can make juice')

class Apple(Fruit):
    def make_juice(self):
        print('I can make apple juice')

class Banana(Fruit):
    def make_juice(self):
        print('I can make banana juice')

class Orange(Fruit):
    def make_juice(self):
        print('I can make orange juice')

# 定义一个公共接口（榨汁机）
def service(obj):
    obj.make_juice()

# 传入不同的对象，产生不同的结果
service(Apple())   # I can make apple juice
service(Banana())  # I can make banana juice
service(Orange())  # I can make orange juice
```



### 2.3 Python 中的多态案例

`+` 运算符就是多态的典型体现：

python

```
# 数值相加 → 算术运算
print(1 + 2)       # 3

# 字符串相加 → 拼接
print('a' + 'b')   # ab

# 列表相加 → 合并
print([1, 2] + [3, 4])  # [1, 2, 3, 4]
```



同一个 `+` 号，对不同类型的数据产生不同的行为。

------

## 三、面向对象其他特性

### 3.1 类属性

| 属性类型     | 定义位置                   | 属于谁       | 用途                   |
| :----------- | :------------------------- | :----------- | :--------------------- |
| **实例属性** | `__init__` 中，`self.属性` | 每个对象独立 | 记录具体对象的特征     |
| **类属性**   | 类中直接定义               | 所有对象共享 | 记录与整个类相关的特征 |

python

```
class Person(object):
    # 类属性（被所有实例共享）
    count = 0
    
    def __init__(self, name):
        self.name = name      # 实例属性
        Person.count += 1     # 每实例化一次，类属性 +1

# 创建对象
p1 = Person('张三')
p2 = Person('李四')
p3 = Person('王五')

# 访问类属性
print(f'共创建了 {Person.count} 个对象')  # 共创建了 3 个对象
```



### 3.2 类方法

> 为什么需要类方法？面向对象强调数据封装性，不建议直接访问类属性，所以通过类方法来操作类属性。

python

```
class Tool(object):
    # 类属性
    count = 0
    
    def __init__(self, name):
        self.name = name
        Tool.count += 1
    
    # 类方法：使用 @classmethod 装饰器
    @classmethod
    def get_count(cls):
        print(f'共创建了 {cls.count} 个工具')

# 使用
t1 = Tool('斧头')
t2 = Tool('榔头')
t3 = Tool('铁锹')
Tool.get_count()  # 共创建了 3 个工具
```



**类方法的特点**：

- 使用 `@classmethod` 装饰器
- 第一个参数是 `cls`（指向类本身）
- 可以访问和修改类属性
- 不需要实例化就可以调用

### 3.3 静态方法

> 如果需要封装一个方法，既不需要访问实例属性/方法，也不需要访问类属性/方法，就可以封装成静态方法。

python

```
class Game(object):
    @staticmethod
    def menu():
        print('1、开始游戏')
        print('2、游戏暂停')
        print('3、退出游戏')

# 直接通过类名调用，无需实例化
Game.menu()
```



**静态方法的特点**：

- 使用 `@staticmethod` 装饰器
- 不需要 `self` 或 `cls` 参数
- 相当于一个普通函数，只是放在类的命名空间中

------

### 3.4 综合案例：游戏类

**需求分析**：

- 设计一个 `Game` 类
- 类属性 `top_score`：记录游戏历史最高分
- 实例属性 `player_name`：记录当前玩家姓名
- 静态方法 `show_help()`：显示游戏帮助信息
- 类方法 `show_top_score()`：显示历史最高分
- 实例方法 `start_game()`：开始当前玩家的游戏

python

```
class Game(object):
    # 类属性：历史最高分
    top_score = 0
    
    def __init__(self, player_name):
        self.player_name = player_name
    
    # 静态方法：显示帮助信息
    @staticmethod
    def show_help():
        print('-' * 40)
        print('【Start】开始游戏')
        print('【Stop】结束游戏')
        print('【Pause】暂停游戏')
        print('-' * 40)
    
    # 类方法：显示历史最高分
    @classmethod
    def show_top_score(cls):
        print(f'本游戏的历史最高分：{cls.top_score}')
    
    # 实例方法：开始游戏
    def start_game(self):
        print(f'{self.player_name} 开始游戏！')

# 调用静态方法（无需实例化）
Game.show_help()

# 调用类方法（无需实例化）
Game.show_top_score()

# 实例化对象，调用实例方法
game = Game('玩家小明')
game.start_game()
```



------

## 知识点总结

### 继承相关速查表

| 概念         | 语法                       | 说明                 |
| :----------- | :------------------------- | :------------------- |
| 单继承       | `class 子类(父类)`         | 一个子类只有一个父类 |
| 多继承       | `class 子类(父类1, 父类2)` | 一个子类有多个父类   |
| 方法重写     | 子类中定义同名方法         | 覆盖父类方法         |
| 调用父类方法 | `super().方法()`           | 在子类中调用父类方法 |

### 类成员速查表

| 类型     | 装饰器          | 第一个参数 | 调用方式                 | 访问范围                   |
| :------- | :-------------- | :--------- | :----------------------- | :------------------------- |
| 实例方法 | 无              | `self`     | 对象.方法()              | 实例属性、类属性、其他方法 |
| 类方法   | `@classmethod`  | `cls`      | 类.方法() 或 对象.方法() | 类属性、其他类方法         |
| 静态方法 | `@staticmethod` | 无         | 类.方法() 或 对象.方法() | 不能访问类/实例成员        |

### 属性类型速查表

| 属性类型 | 定义方式         | 访问方式                 | 数据范围     |
| :------- | :--------------- | :----------------------- | :----------- |
| 实例属性 | `self.属性 = 值` | `对象.属性`              | 每个对象独立 |
| 类属性   | 类中直接定义     | `类.属性` 或 `对象.属性` | 所有对象共享 |

------

## 练习题

1. **单继承**：定义一个 `Animal` 类，有 `eat` 方法；定义 `Dog` 和 `Cat` 类继承自 `Animal`，并重写 `call` 方法
2. **多继承**：定义一个 `Flyable` 类（有 `fly` 方法）和 `Swimmable` 类（有 `swim` 方法），再定义一个 `Duck` 类同时继承这两个类
3. **类属性**：定义一个 `Student` 类，用类属性记录创建了多少个学生对象
4. **类方法与静态方法**：定义一个 `Calculator` 类，包含静态方法 `add`、`sub` 和类方法 `get_pi`

![01面向对象](E:\黑马学习资料\各种文件\面试\01_python进阶\00总结图片\01面向对象.png)