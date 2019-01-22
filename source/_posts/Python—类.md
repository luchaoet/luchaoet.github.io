---
title: Python—类
date: 2019-01-17 22:27:20
tags: ['Python', '类']
summary:
---
面向对象编程是最有效的软件编写方法之一。在面向对象编程中，你编写表现现实世界中的事物和情景的类，并基于这些类来创建对象。编写类时，你定义一大类对象都有的通用行为。基于类创建对象时，每个对象都自动具备这种通用行为，然后可根据需要赋予每个对象独特的个性<br />根据类来创建对象被称为实例化

根据Dog类创建的每个实例都将存储名字和年龄。赋予每条小狗说狗语（sayName()）的能力
```python
class Dog:
	def __init__(self,name,age):
		self.name = name
		self.age = age
    
	def sayName(self):
		print('my name is ' + self.name + ', I am ' + str(self.age) + ' years old')
```

### __init__()方法
每当创建实例时，都会自动运行这个方法。开头末尾都有__两个下划线，这是一种约定，用于区别普通方法，避免冲突<br />形参`self`必不可少，且必须位于其他参数前面，它是一个指向实例本身的引用，让实例能够访问类中的属性和方法。

### 根据类创建实例
```python
mydog = Dog('lucy',20)
# 调用方法
mydog.sayName() # my name is lucy, I am 20 years old
# 访问属性
print(mydog.name) # lucy
```

### 继承
一个类继承另一个类时，它将自动获得另一个类的所有属性和方法。原有的类称为父类，新的类称为子类。子类继承了父类的所有方法和属性，同时还可以定义自己的属性和方法
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0
    
    def get_descriptive_name(self):
        return str(self.year) + ' ' + self.make + ' ' + self.model
    
    def read_odometer(self):
        print('self.odometer_reading = ' + self.odometer_reading)
    
    def fill_gas_tank(self):
        print('油加满')


class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
```

定义子类时，需要在括号内指定父类的名称。子类__init__()方法接受创建Car实例所需要的信息<br />super()函数，将父类和子类关联起来。调用父类的__init__方法让ElectricCar实例包含父类的所有属性。父类也成为超类，super因此而来

#### 子类的__init__()方法
创建子类实例时，Python首先需要给父类所有属性赋值，为此需要调用父类的__init__方法

#### 给子类定义属性和方法
```python
class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery_size = 70
    
    def describe_battery(self):
        print('This Car has a ' + str(self.battery_size) + '-KWh battery.')
```

ElectricCar类添加了新属性 self.battery_size，并设置初始值为70。ElectricCar类包含这个属性，Car类不包含该属性<br />ElectricCar类还添加了describe_battery()方法

#### 重写父类的方法
Car类有一个名为fill_gas_tank()的方法，如果子类中不需要这个方法，可以在子类中重写该方法，父类中的该方法将被子类中的该方法覆盖
```python
class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery_size = 70
    
    def describe_battery(self):
        print('This Car has a ' + str(self.battery_size) + '-KWh battery.')
    
    def fill_gas_tank(self):
        print('我不需要油，我要电')
```

#### 将实例用作属性
```python
class Battery():
    def __init__(self, battery_size=70):
        self.battery_size = battery_size
    
    def describe_battery(self):
        print('battery_size = ' + str(self.battery_size))
 

 class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery_size = 70
        self.battery = Battery()
     
my_tesla = ElectricCar('tesla', 'model s', 2016)
my_tesla.battery.describe_battery() # battery_size = 70
```

定义一个Battery类，将它的实例赋值给ElectricCar类中的battery属性，即将实例存储在属性self.battery中。每当__init__()方法被调用时，都将执行该操作，因此每隔ElectricCar实例都包含一个自动创建的Battery实例

### 导入类
Python允许将类存储在模块中，然后在主程序中导入所需的模块
#### 导入单个类
car.py
```python
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0
    
    def get_descriptive_name(self):
        return str(self.year) + ' ' + self.make + ' ' + self.model
    
    def read_odometer(self):
        print('self.odometer_reading = ' + self.odometer_reading)
    
    def fill_gas_tank(self):
        print('油加满')
```

my_car.py
```python
from car import Car

my_car = Car('audi', 'a4', 2016)
my_car.read_odometer() # self.odometer_reading = 0
```

#### 在一个模块中存储多个类
car.py
```python
class Battery():
    def __init__(self, battery_size=70):
        self.battery_size = battery_size

class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery_size = 70
```

#### 从一个模块中导入多个类
```python
from car import Car, Battery
```

#### 导入整个模块
```python
import car
my_car = car.Car('audi', 'a4', 2016)
```

#### 导入模块中的所有类
```python
from car import *
```

#### 在一个模块中导入另一个模块
```python
from car import Car
class ElectricCar(Car):
    def __init__(self, make, model, year):
      pass
```

### 类的编码风格
1. 类名采用驼峰命名法，类名的每个单词都大些，而不是使用下划线。实例名和模块名都采用小些格式，并在单词之间加上下划线
1. 对于每个类，都应该在类定义后面包含一个文档字符串，这种文档字符串简要的描述类的功能，并遵循编写函数的文档字符串时采用的格式约定
1. 采用一个空行分隔方法，采用两个空行来分隔类
1. 需要同时导入标准库的模块和自己编写的模块时，先编写导入标准库模块的import语句，在加上空行，然后编写导入自己编写的模块的import语句