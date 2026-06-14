# Python 面向对象编程

> 面向对象是一种编程思想，它让代码更贴近现实世界的思维方式。本节将从零开始，带你理解并掌握 Python 中的面向对象编程。

------

## 一、面向对象编程思想

### 1.1 什么是面向过程？

传统的面向过程编程思想总结起来就是八个字：**自顶向下，逐步细化**。

- 将要实现的功能描述为一个从开始到结束、按部就班的连续步骤
- 依次逐步完成这些步骤
- 如果某个步骤难度较大，可以将其再次细分为若干个子步骤
- 以此类推，直到得到我们想要的结果

**举个例子**：大家去考证书的过程

> 学生报名 → 提供身份证明材料 → 缴纳考试费用 → 获得凭证 → 参加考试 → 取得成绩

面向过程就是一步一步、非常详细地去执行每个环节。

### 1.2 什么是面向对象？

在面向过程的分析中，我们关注的是「步骤」。而面向对象关注的是：**谁**来完成这些步骤？

- 报名 → 学生报名
- 提供材料 → 学生提供材料
- 登记信息 → 老师登记信息

**核心思想**：任何一个功能的实现，都可以看作是一个个实体在发挥各自的「能力」，并在内部进行协调有序的调用。

### 1.3 一个生动的比喻

| 编程思想     | 比喻                               |
| :----------- | :--------------------------------- |
| **面向过程** | 像员工，什么事都要亲力亲为         |
| **面向对象** | 像老板，想清楚流程后，安排别人去做 |

### 1.4 面向对象的核心概念

**属性**：实体固有的特征信息（在面向对象术语中，就是以前的变量）

- 一个人的属性：姓名、年龄、身高、体重、身份证号、毕业学校
- 一部手机的属性：价格、品牌、操作系统、颜色、尺寸

**功能**：实体可以完成的动作（在面向对象术语中，功能就是封装成的方法）

### 1.5 面向对象 vs 面向过程

| 对比维度     | 面向过程 | 面向对象       |
| :----------- | :------- | :------------- |
| 思维方式     | 步骤分解 | 实体协作       |
| 代码重用     | 函数     | 类+对象        |
| 模块化程度   | 一般     | 更深           |
| 数据安全     | 一般     | 更封闭、更安全 |
| 大型复杂业务 | 较难维护 | 更容易解决     |
| 前期开发     | 简单     | 较复杂         |
| 维护扩展     | 较难     | 简单           |
| 执行效率     | 较高     | 稍低           |

> 💡 **结论**：面向对象不是一种技术，而是一种思想，是一种解决问题的思维方式。两者各有优劣，应根据实际场景选择。

------

## 二、面向对象的基本概念

### 2.1 类与对象

| 概念               | 说明                                     | 类比                 |
| :----------------- | :--------------------------------------- | :------------------- |
| **类（Class）**    | 具有相同或相似属性和动作的一组实体的集合 | 模具、蓝图           |
| **对象（Object）** | 现实中的一个具体实体                     | 用模具生产出来的产品 |

**关系**：对象由类产生，类规定了对象有哪些属性和方法。

### 2.2 类的定义

python

```
# Python 3 中定义类的方式
class 类名(object):
    # 属性
    # 方法
```



**命名规范**：

- 类名使用**大驼峰命名法**（每个单词首字母大写）
- 例如：`Person`、`StudentInfo`、`CarFactory`

python

```
# 定义一个简单的类
class Person(object):
    # 方法（函数）
    def eat(self):
        print('吃零食')
    
    def drink(self):
        print('喝可乐')
```



### 2.3 类的实例化（创建对象）

> **实例化**：通过类得到对象的过程。类本身基本什么都做不了，必须通过实例化得到对象才能使用。

python

```
# 语法：对象名 = 类名()

# 实例化 Person 类得到对象 p1
p1 = Person()

# 调用对象的方法
p1.eat()    # 吃零食
p1.drink()  # 喝可乐

# 可以创建多个对象
p2 = Person()
p2.eat()
```



### 2.4 `self` 关键字

> `self` 是 Python 内置的关键字，指向**类实例对象本身**。

python

```
class Person():
    def speak(self):
        print(f"我是：{self}")

p1 = Person()
print(p1)      # <__main__.Person object at 0x...>
p1.speak()     # 我是：<__main__.Person object at 0x...>

p2 = Person()
print(p2)      # 不同的内存地址
p2.speak()     # 我是：<__main__.Person object at 0x...>
```



**一句话总结**：`self` 就是谁实例化了对象，它就指向谁。

------

## 三、对象的属性添加与获取

### 3.1 什么是属性？

属性即特征，比如：

- 人的姓名、年龄、身高、体重
- 车的品牌、型号、颜色、载重量

对象属性既可以在**类外面**添加和获取，也可以在**类里面**添加和获取。

### 3.2 在类的外面添加和获取属性

python

```
# 1. 定义一个 Person 类
class Person(object):
    pass

# 2. 实例化对象
p1 = Person()

# 3. 为对象添加属性（类的外部）
p1.name = '孙悟空'
p1.age = 500
p1.address = '花果山水帘洞'

# 4. 获取对象属性
print(f'姓名：{p1.name}')   # 姓名：孙悟空
print(f'年龄：{p1.age}')     # 年龄：500
```



### 3.3 在类的内部获取属性

python

```
class Person():
    def speak(self):
        print(f'我的名字：{self.name}，我的年龄：{self.age}')

# 实例化对象
p1 = Person()
p1.name = '孙悟空'
p1.age = 500
p1.speak()   # 我的名字：孙悟空，我的年龄：500
```



> ⚠️ **问题**：这种方式每次都需要手动为对象添加属性，比较繁琐。我们希望在实例化对象时就能直接设置属性。

------

## 四、魔术方法

> 魔术方法（Magic Methods）是 Python 中具有**特殊功能**的方法，格式为 `__方法名__()`，它们有自己的触发条件。

### 4.1 `__init__()` 方法（初始化方法/构造方法）

**作用**：在实例化对象时自动触发，用于初始化对象的属性。

python

```
class Person():
    # 初始化方法：实例化对象时自动调用
    def __init__(self, name, age):
        self.name = name   # 将参数赋值给对象属性
        self.age = age

# 实例化对象时直接传入属性值
p1 = Person('孙悟空', 500)
print(p1.name)   # 孙悟空
print(p1.age)    # 500
```



### 4.2 `__str__()` 方法

**作用**：当使用 `print(对象)` 输出对象时自动调用。必须返回一个**字符串**。

python

```
class Car():
    def __init__(self, brand, model, color):
        self.brand = brand
        self.model = model
        self.color = color
    
    def __str__(self):
        return f'汽车品牌：{self.brand}，型号：{self.model}，颜色：{self.color}'

# 实例化对象
c1 = Car('奔驰', 'S600', '黑色')
print(c1)   # 汽车品牌：奔驰，型号：S600，颜色：黑色
```



### 4.3 `__del__()` 方法（析构方法）

**作用**：当删除对象时（调用 `del` 或文件执行结束后）自动触发。常用于关闭文件、关闭数据库连接等。

python

```
class Person():
    def __init__(self, name):
        self.name = name
    
    def __del__(self):
        print(f'{self.name} 对象已被删除')

p1 = Person('张三')
del p1   # 张三 对象已被删除
```



### 4.4 魔术方法总结

| 魔术方法         | 触发时机         | 主要作用                                   |
| :--------------- | :--------------- | :----------------------------------------- |
| `__init__(self)` | 实例化对象时     | 初始化对象属性                             |
| `__str__(self)`  | `print(对象)` 时 | 自定义对象的输出内容（必须 return 字符串） |
| `__del__(self)`  | 删除对象时       | 关闭文件、关闭数据库连接等                 |

------

## 五、面向对象的三大特性

面向对象编程有三大特性：**封装、继承、多态**。本节先介绍封装。

### 5.1 什么是封装？

封装有两层含义：

1. **数据封装**：把现实世界中的主体的属性和方法书写到类的里面
2. **访问控制**：为属性和方法添加私有权限，不能被外部直接访问

python

```
class Person():
    # 封装属性和方法
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        print(f"我是{self.name}")
```



### 5.2 私有属性和私有方法

有时我们不希望在类的外部访问某些属性或方法，可以将其设置为**私有**。

**语法**：在属性名或方法名前加上**两个下划线 `__`**

python

```
class Girl():
    def __init__(self, name):
        self.name = name
        self.__age = 18      # 私有属性
    
    def __secret(self):      # 私有方法
        print("这是一个秘密")
    
    # 通过公共方法访问私有属性
    def get_age(self):
        return self.__age
    
    def set_age(self, age):
        if 0 < age < 150:
            self.__age = age
        else:
            print("年龄无效")

girl = Girl('小美')
print(girl.name)        # 小美
# print(girl.__age)     # 报错！私有属性不能直接访问
print(girl.get_age())   # 18（通过公共接口访问）

girl.set_age(19)
print(girl.get_age())   # 19
```



### 5.3 封装的意义

1. **明确区分内外**：控制外部对隐藏属性的操作行为
2. **数据过滤**：在设置属性时进行有效性验证

python

```
class People():
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
    
    def tell_info(self):
        print(f'姓名：{self.__name}，年龄：{self.__age}')
    
    def set_info(self, name, age):
        # 数据验证
        if not isinstance(name, str):
            print('名字必须是字符串类型')
            return
        if not isinstance(age, int):
            print('年龄必须是数字类型')
            return
        self.__name = name
        self.__age = age

p = People('张三', 25)
p.tell_info()
p.set_info('李四', 30)
p.tell_info()
```



1. **简化程序复杂度**：将复杂操作封装在内部，对外只暴露简单接口

python

```
class ATM():
    def __card(self):
        print('插卡')
    def __auth(self):
        print('用户认证')
    def __input(self):
        print('输入取款金额')
    def __print_bill(self):
        print('打印账单')
    def __take_money(self):
        print('取款')
    
    # 对外提供的公共方法
    def withdraw(self):
        self.__card()
        self.__auth()
        self.__input()
        self.__print_bill()
        self.__take_money()

atm = ATM()
atm.withdraw()   # 只需调用一个方法，内部自动完成所有步骤
```



------

## 六、实战案例

### 案例1：学生成绩管理

**需求**：定义学员信息类，包含姓名、成绩属性，定义成绩打印方法。

- 90分及以上：优秀
- 80分及以上：良好
- 70分及以上：中等
- 60分及以上：合格
- 60分以下：不及格

python

```
class Student():
    def __init__(self, name, score):
        self.name = name
        self.score = score
    
    def print_grade(self):
        if self.score >= 90:
            grade = '优秀'
        elif self.score >= 80:
            grade = '良好'
        elif self.score >= 70:
            grade = '中等'
        elif self.score >= 60:
            grade = '及格'
        else:
            grade = '不及格'
        print(f'姓名：{self.name}，成绩：{self.score}，等级：{grade}')

# 测试
tom = Student('Tom', 80)
tom.print_grade()      # 姓名：Tom，成绩：80，等级：良好

jennifer = Student('Jennifer', 59)
jennifer.print_grade() # 姓名：Jennifer，成绩：59，等级：不及格
```



### 案例2：小明减肥

**需求**：小明体重 75.0 公斤，每次跑步减重 0.1 公斤，每次吃东西增重 0.2 公斤。

python

```
class Person():
    def __init__(self, name, weight):
        self.name = name
        self.weight = weight
    
    def __str__(self):
        return f'姓名：{self.name}，体重：{self.weight} KG'
    
    def run(self):
        """跑步减肥"""
        self.weight -= 0.1
        print(f'{self.name}跑完步，体重变为{self.weight} KG')
    
    def eat(self):
        """吃饭增重"""
        self.weight += 0.2
        print(f'{self.name}吃完饭，体重变为{self.weight} KG')

# 实例化对象
xiaoming = Person('小明', 75.0)
print(xiaoming)

# 吃饭
xiaoming.eat()
# 跑步减肥
xiaoming.run()
```



------

## 知识点总结

### 类的定义与实例化

python

```
# 定义类
class 类名(object):
    def __init__(self, 参数):
        self.属性 = 参数
    
    def 方法(self):
        # 方法体
        pass

# 实例化对象
对象 = 类名(参数)
```



### 魔术方法速查表

| 魔术方法         | 触发时机         | 返回值要求         |
| :--------------- | :--------------- | :----------------- |
| `__init__(self)` | 实例化对象时     | 无                 |
| `__str__(self)`  | `print(对象)` 时 | 必须 return 字符串 |
| `__del__(self)`  | 删除对象时       | 无                 |

### 私有成员

python

```
class Demo():
    def __init__(self):
        self.__private_attr = 10   # 私有属性
        self.public_attr = 20      # 公有属性
    
    def __private_method(self):    # 私有方法
        pass
    
    def public_method(self):       # 公有方法
        return self.__private_attr
```



### 封装的意义

| 作用     | 说明                                   |
| :------- | :------------------------------------- |
| 数据保护 | 私有属性不能直接访问，需通过公共接口   |
| 数据过滤 | 在 `set_xx` 方法中添加数据验证逻辑     |
| 简化接口 | 复杂操作封装在内部，对外只暴露简单方法 |

------

## 练习题

1. **定义类**：定义一个 `Book` 类，包含书名、作者、价格属性，实现 `__str__` 方法
2. **私有属性**：定义一个 `BankAccount` 类，包含账户名和余额（私有），实现存款、取款、查询余额的方法
3. **学生成绩**：完善学生成绩案例，添加一个方法计算平均分
4. **小狗类**：定义一个 `Dog` 类，包含名字、品种属性，实现叫、跑、吃三个方法