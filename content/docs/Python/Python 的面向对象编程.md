---
title: Python 的面向对象编程
date: 2025-09-09T17:28:37+08:00
author: LiangMingJian
---

# 基本概念

面向对象编程（Object Oriented Programming，OOP）是一种常用的程序设计思想。它要求将对象作为程序的基本单元，一个对象包括了数据属性和操作数据的方法，一堆具有相同数据属性和操作方法的对象就会被抽象为一个类。

**简单来说，类（Class）就是工厂生产过程中的模具，对象（Object）就是使用模具生产出的零件，对象又称为类的实例（Instance）。**

**面向对象编程的三个基本特征是继承、多态和封装。**

- 继承：允许子类继承父类的属性和方法，实现代码复用和层次分类。
- 多态：同一操作数据方法支持通过重写或重载在不同对象中产生不同结果。
- 封装：将数据和操作数据的方法绑定在一起，隐藏内部实现细节，只暴露必要接口。

> **对象 Object**：在 Python 中，所有的数据类型，值，变量，函数，类，实例等一切可操作的基本单元是对象，每个对象有三个基本属性：ID，类型和值。

> **实例 Instance**：实例和对象是一个东西，说法的不同主要是应用语境的不同。对一般数据，往往称之为对象，对于类的实现，则称之为实例。

> **类 Class**：类是相似的一组对象的抽象，它能通过代码描述怎么实现这一组对象中所有的成员，并具备这组对象中所有相同的数据属性和操作方法。

# 类的实现

通过关键字 `class` 可以定义一个类。然后在对类进行实例时，只需调用函数一样即可。

```python
# 创建一个 People 类
class People:
    pass

# 创建一个 People 类的实例
teacher = People()
```

# 类的属性

一个类是可以具有自己的属性的，这个支持的属性又称为类属性。

**类属性支持通过类名访问，也支持通过实例访问。另外，类属性的赋值支持在类定义时赋值，也支持在实例化时赋值**。

通过构建**实例化方法** `__init__()` 可以完成诸如类属性赋值，类初始化等工作。

```python
class People:  
    version = 'v1.0'  
  
    def __init__(self, name, age):  
        self.name = name  
        self.age = age  
  
teacher = People('Jack', 20) 
print(f'版本: {People.version}')  # 版本: v1.0
print(f'姓名: {teacher.name}')     # 姓名: Jack
print(f'年龄: {teacher.age}')      # 年龄: 20
```

此外，**类属性分为公有属性和私有属性两种**。公有属性允许在类实例化之后访问和修改，私有属性则禁止在类实例化后访问和修改，只能由类里面的方法进行访问修改。**两种属性的区分通过是否在变量名前具有两个下划线** `__`。

```python
class People:  
    __version = 'v1.0'  
  
    def __init__(self, name, age):  
        self.name = name  
        self.age = age  
  
    def get_version(self):  
        return self.__version  
  
teacher = People('Jack', 20)  

# 类的公有属性支持修改和访问
teacher.age = 21  
print(f'年龄: {teacher.age}')  # 年龄: 21

# 类的私有属性只能由类方法进行访问和修改，不能直接通过实例访问
print(f'版本: {teacher.get_version()}')  # 版本: v1.0
print(f'版本: {teacher.__version}')  
# AttributeError: 'People' object has no attribute '__version'
```

特别的，**类实例后的对象是允许动态新增属性和方法的**，但一般不建议这么使用。

```python
from types import MethodType  
  
class People:  
    __version = 'v1.0'  
  
    def __init__(self, name, age):  
        self.name = name  
        self.age = age  

teacher = People('Jack', 20)  
  
# 给实例对象添加属性
teacher.height = 180  

# 给实例对象添加属性
def get_height(self):  
    print(f"身高: {self.height}")  
teacher.get_height = MethodType(get_height, teacher)  

# 支持调用
teacher.get_height()   # 身高: 180
print(teacher.height)  # 180
```

最后，**类提供一个特殊变量 `__slots__` 用以限制类属性和方法**。

`__slots__` 是一个字符串元组，它要求类所有的属性名必须在其中声明，没有声明的属性名，禁止被生成，包括实例对象的动态生成（可以不包括类定义的属性）。

此外，在定义特殊变量 `__slots__` 后，所有类方法都会变为只读禁止实例化时重写，也禁止实例化之后动态生成。

```python
class People:  
    __slots__ = ('name', 'age')  
    __version = 'v1.0'  # 类定义的属性允许不声明 
  
    def __init__(self, name, age, ids):  
        self.name = name  
        self.age = age  
        self.id = ids  # 这个属性没有被声明，禁止使用
  
    def get_version(self):  
        return self.__version  
  
teacher = People('Jack', 20)
teacher.height = 30  # 这个属性也禁止被添加
teacher.get_height = MethodType(get_height, teacher)  # 这个方法也禁止被添加
```

# 类的方法

类方法是指哪些调用了类属性来完成相关功能的方法，它是类各种功能的接口。

**一个标准类方法的第一个参数必定是 `self`，它指代类自身，在调用时被隐式传递，主要作用是用于获取类属性。**

```python
class People:  
    __version = 'v1.0'  
  
    def __init__(self, name, age):  
        self.__name = name  
        self.__age = age  
  
    # 类方法
    def get_version(self):  
        return self.__version  
  
    def get_name(self):  
        return self.__name  
  
    def get_age(self):  
        return self.__age  
  
teacher = People('Jack', 20)  
print(f'版本: {teacher.get_version()}')  # 版本: v1.0  
print(f'姓名: {teacher.get_name()}')     # 姓名: Jack  
print(f'年龄: {teacher.get_age()}')      # 年龄: 20
```

**特别的，Python 提供装饰器 `@staticmethod`，`@classmethod` 和 `@property` 来实现一些特别的类方法定义。**

`@staticmethod` 实现不调用类属性，并支持不创建类实例就可以直接通过类名调用的类静态方法。

```python
class People:  
  
    @staticmethod  
    def static():  
        print("hello")  

# 直接类名调用
People.static()   # hello v1.0

# 当然，实例化之后调用也支持
teacher = People()  
teacher.static()  # hello v1.0
```

`@classmethod` 实现可调用类属性，并·支持不创建类实例就可以直接通过类名调用的类的类方法。

特别的，使用该装饰器的方法的第一个参数必定是 `cls`，与 `self` 进行区分，但同样代表类自身。

```python
class People:  
    __version = 'v1.0'  
  
    @classmethod  
    def static(cls):  
        print(f"hello {cls.__version}")  

# 直接类名调用
People.static()   # hello v1.0

# 实例化之后也支持
teacher = People()  
teacher.static()  # hello v1.0
```

`@property` 实现实例方法的的属性化，可以将对私有属性的获取，设置，删除方法封装为一个类属性，然后直接通过类属性调用对应方法。

注意，`@property` 需要指向获取方法，然后记录获取方法，比如为 `argopt`。然后需要用获取方法的名称结合 `setter` 如 `@argopt.setter` 指向设置方法。接着使用 `@argopt.deleter` 指向删除方法。同时，所有指向的方法名必须相同。

```python
class People:  
    __arg = 0  
  
    @property  
    def argopt(self):  
        return self.__arg  
  
    @argopt.setter  # 装饰器是获取函数名
    def argopt(self, value):  
        self.__arg = value  
  
    @argopt.deleter  # 装饰器是获取函数名
    def argopt(self):  
        del self.__arg
  
teacher = People()  

# 通过类属性调用 setarg 方法
teacher.arg = 21  
# 通过类属性调用 getarg 方法
print(teacher.arg)  
# 通过类属性调用 delarg 方法
del teacher.arg
```

最后，Python 为类提供了一些默认的属性和方法，以便更好的使用类。

常用的内置属性和方法有：`__doc__`（类文档），`__str__()`（类默认输出），`__call__()`（类默认执行）。

```python
class People:  
    """  
    这里是类的描述  
    """  
    def __str__(self):  
        return '我是默认输出'  
  
    def __call__(self, *args, **kwargs):  
        print('我是默认执行')  
        return 0  
  
teacher = People()  
print(teacher.__doc__)  # 这里是类的描述
print(teacher)         # 我是默认输出
print(teacher())      # 我是默认执行 和 0
```

# 类的继承

继承是面向对象编程的一大特征，继承可以使得子类具有父类的属性和方法，并允许对属性和方法进行扩展。

如下述代码，通过继承类 People，类 Teacher 和 Student 复用了类 People 的属性和方法，并自行扩展了更多的属性。

```python
class People:  
  
    def __init__(self, name, age):  
        self.name = name  
        self.age = age  

# 继承 People 类
class Teacher(People):  
  
    def __init__(self, name, age, major, school):  
        super().__init__(name, age)  
        self.major = major  
        self.school = school  

# 继承 People 类
class Student(People):  
  
    def __init__(self, name, age, class_id):  
        super().__init__(name, age)  
        self.class_id = class_id  
  
teacher = Teacher("John", 21, "Math", 23)  
student = Student("Tony", 10, 1)
```

除了常规的单个继承，Python 还提供多重继承的方法，让一个类同时继承多个类。

```python
class People:  
  
    def __init__(self, name, age):  
        self.name = name  
        self.age = age  
  
class School:  
  
    def __init__(self, school_name, employee_id):  
        self.school_name = school_name  
        self.employee_id = employee_id  

# 同时继承 People 和 School 类
class Teacher(People, School):  
  
    def __init__(self, name, age, school_name, employee_id, major):  
        People.__init__(self, name, age)  
        School.__init__(self, school_name, employee_id)  
        self.major = major  
  
teacher = Teacher("Jack", 35, "MyShcool", "100", "Math")
```
