#### 类和对象

创建类和对象：

```python
class Turtle:   #类
    head = 1
    eyes = 2
    legs = 4
    shell = True

    def crawl(self): #self是对象本身
        ...


t1 = Turtle()   #对象
t1.head = 3   #修改值
t1.mouth = 1  #没有属性就创建属性
```

##### 继承：

具有上下从属关系

将父类写在子类类名后面的小括号中

如果在子类里面，存在跟父类一样的属性和方法名，那么子类就会覆盖父类

```python
>>> class A:  #父类
        x = 520
        def hello(self):
            print("你好，我是A~")

>>> class B(A):  #子类
        x = 880
        def hello(self):
            print("你好，我是B~")

>>> b = B()
>>> b.x
880
>>> b.hello()
你好，我是B~
```

###### isinstance() 和 issubclass()

isinstance() 判断一个对象是否属于某个类

issubclass() 判断一个类是否属于某个类的子类

```python
>>> class A:
        pass

>>> class B(A):
        pass

>>> b = B()
>>> isinstance(b, B)
True
>>> isinstance(b, A)
True
>>> type(b)
<class '__main__.B'>
```

###### 多重继承：

一个子类同时可以继承多个父类

```python
>>> class A:
        x = 520
        def hello(self):
            print("你好，我是A~")

>>> class B:
        x = 880
        y = 250
        def hello(self):
            print("你好，我是B~")

>>> class C(A, B):
        pass

>>> issubclass(C, A)
True
>>> issubclass(C, B)
True
>>> c = C()
>>> c.x
520
>>> c.y
250
>>> c.hello()
你好，我是A~
```

多个父类拥有相同属性和方法的情况，它的访问顺序是按从左到右的

###### 组合：

多个类则是同级关系

```python
>>> class Turtle:
        def say(self):
            print("不积跬步，无以至千里！")
        
>>> class Cat:
        def say(self):
            print("喵喵喵~")
        
>>> class Dog:
        def say(self):
            print("哟吼，我是一只小狗~")
        
>>> class Garden:
        t = Turtle()
        c = Cat()
        d = Dog()
        def say(self):
            self.t.say()
            self.c.say()
            self.d.say()
                      
>>> g = Garden()
>>> g.say()
不积跬步，无以至千里！
喵喵喵~
哟吼，我是一只小狗~
```

###### 绑定

 self 起到的作用 —— 绑定

实例对象跟类的方法进行绑定

只要通过绑定，就可以实现各个对象设置各个对象的属性

```python
>>> class C:
        x = 100
        def set_x(self, v):
            self.x = v

>>> c1 = C()
>>> c2 = C()
>>> c1.x
100
>>> c2.x
100
>>> c1.set_x(250)
>>> c1.x
250
>>> c2.set_x(520)
>>> c2.x
520
# 注意：如果对象同名属性未设置，会共享使用类的属性。
>>> c3 = C()
>>> c3.x
100
>>> c3.__dict__
{}
>>> c3.set_x(666)
>>> c3.x
666
>>> c3.__dict__
{'x': 666}
```

###### 只有一个 pass 语句填充的类

什么都没有的类，可以把它当字典来使用

```python
>>> class C:
        pass

>>> c = C()
>>> c.x = 250
>>> c.y = "小甲鱼"
>>> c.z = [1, 2, 3]
>>> print(c.x)
250
>>> print(c.y)
小甲鱼
>>> print(c.z)
[1, 2, 3]
```

##### 构造函数：

在类中定义 \_\_init__()

```python
>>> class C:
        def __init__(self, x, y):
            self.x = x
            self.y = y
        def add(self):
            return self.x + self.y
        def mul(self):
            return self.x * self.y

>>> c = C(2, 3)
>>> c.add()
5
>>> c.mul()
6
```

##### 重写

如果对于父类的某个属性或方法不满意的话，可以重新写一个同名的属性或方法对其进行覆盖

```python
>>> class D(C):
        def __init__(self, x, y, z):
            C.__init__(self, x, y)
            self.z = z
        def add(self):
            return C.add(self) + self.z
        def mul(self):
            return C.mul(self) * self.z

>>> d = D(2, 3, 4)
>>> d.add()
9
>>> d.mul()
24
```

##### 钻石继承（菱形继承）

类 B1 和 类 B2 都是继承自同一个父类 A，而类 C 又同时继承自它们（处理不好，容易出现问题）

```python
>>> class A:
        def __init__(self):
            print("哈喽，我是A~")
        
>>> class B1(A):
        def __init__(self):
            A.__init__(self)
            print("哈喽，我是B1~")
        
>>> class B2(A):
        def __init__(self):
            A.__init__(self)
            print("哈喽，我是B2~")
        
>>> class C(B1, B2):
        def __init__(self):
            B1.__init__(self)
            B2.__init__(self)
            print("哈喽，我是C~")
        
>>> c = C()
哈喽，我是A~
哈喽，我是B1~
哈喽，我是A~
哈喽，我是B2~
哈喽，我是C~
```

类 A 的构造函数被调用了 2 次

###### super() 函数和 MRO 顺序

通过类名直接访问的做法，是有一个名字的，叫 “调用未绑定的父类方法”

遇到钻石继承嘛，就容易出事

一个更好的实现方案，就是使用 super() 函数

super() 函数能够在父类中搜索指定的方法，并自动绑定好 self 参数，按照 MRO （方法解析顺序）顺序去搜索方法

```python
>>> class B1(A):
        def __init__(self):
           super().__init__()
           print("哈喽，我是B1~")
        
>>> class B2(A):
        def __init__(self):
            super().__init__()
            print("哈喽，我是B2~")
        
>>> class C(B1, B2):
        def __init__(self):
            super().__init__()
            print("哈喽，我是C~")
        
>>> c = C()
哈喽，我是A~
哈喽，我是B2~
哈喽，我是B1~
哈喽，我是C~
```

查找一个类的 MRO 顺序有两种方法

```python
#1.mro()
>>> C.mro()
[<class '__main__.C'>, <class '__main__.B1'>, <class '__main__.B2'>, <class '__main__.A'>, <class 'object'>]
>>> B1.mro()
[<class '__main__.B1'>, <class '__main__.A'>, <class 'object'>] 
#2.__mro__属性
>>> C.__mro__
(<class '__main__.C'>, <class '__main__.B1'>, <class '__main__.B2'>, <class '__main__.A'>, <class 'object'>)
>>> B2.__mro__
(<class '__main__.B2'>, <class '__main__.A'>, <class 'object'>)
```

##### Mixin (Mix-in)

一种设计模式

针对面向对象开发过程中，反复出现的问题，而设计出来的解决方案

##### 类方法

类中定义的函数

绑定到类和类的实例上的函数，通常与类实例的属性一起使用

类方法的第一个参数是 cls，表示类本身，而不是类的实例

类方法可以通过 @classmethod 装饰器定义，并且它们通常用于修改类级别的属性，而不是实例级别的属性

不管是 self 参数，还是 cls 参数，都是一种约定俗成的叫法，可以使用其他参数名代替的

```python
>>> class C:
        def funA(self):
            print(self)
        
        @classmethod
        def funB(cls):
            print(cls)
        
>>> c = C()
>>> c.funA()
<__main__.C object at 0x0000021DEC502D30>
>>> c.funB()
<class '__main__.C'>
```

特点就是绑定的是类，而非实例对象

可能需要一个类能够统计其实例化对象的数量

```python
#这里get_count() 由于是类方法
#就算实例属性覆盖了类属性，也不会有影响
>>> class C:
        count = 0
        def __init__(self):
            C.count += 1
        
        @classmethod
        def get_count(cls):
            print(f"该类一共实例化了 {cls.count} 个对象。")
        
>>> c1 = C()
>>> c2 = C()
>>> c3 = C()
>>> c3.get_count()
该类一共实例化了 3 个对象。
>>> c3.count = 1
>>> c3.get_count()
该类一共实例化了 3 个对象。
```

##### 静态方法：

 “放在类里的普通函数”，它与类本身或类的实例没有直接关联

不依赖类的实例属性，也不依赖类的属性，它通常用于逻辑上与类相关的操作，但这些操作并不需要访问或修改类或实例的状态

```python
>>> class C:
        count = 0
        def __init__(self):
            C.count += 1
        
        @staticmethod
        def get_count():
            print(f"该类一共实例化了 {C.count} 个对象。")
        
>>> c1 = C()
>>> c2 = C()
>>> c3 = C()
>>> c3.get_count()
该类
一共实例化了 3 个对象。
#不用担心对象覆盖统计属性的问题
>>> c3.count = 1
>>> c3.get_count()
该类一共实例化了 3 个对象。
```

###### 静态方法 vs 类方法 vs 实例方法

实例方法：  

第一个参数是 self，表示实例本身。

可以访问和修改实例属性。  

类方法：  

第一个参数是 cls，表示类本身。

可以访问类属性，但不能访问实例属性。

使用 @classmethod 装饰。  

静态方法：  

没有 self 或 cls 参数。

不能访问实例属性或类属性。

使用 @staticmethod 装饰。

###### **什么时候使用类方法，什么时候使用静态方法？**

静态方法适合用于那些与类或实例的状态无关的操作，而类方法更适合处理需要访问或管理类级别的属性的任务，比如统计实例对象数量这样的需求。

##### 描述符

实现了属性访问协议的类

本质是在一个类中定义了特殊方法 \_\_get__()、\_\_set__() 和 \_\_delete__() 之一或全部

当一个描述符的实例被作为另一个类的类属性时，就可以拦截对该属性的访问

启用：

在另外一个类里面，将它的实例化对象赋值给想要管理的属性

```python
>>> class D:
        def __get__(self, instance, owner):
            print(f"get~\nself -> {self}\ninstance -> {instance}\nowner -> {owner}")
        def __set__(self, instance, value):
            print(f"set~\nself -> {self}\ninstance -> {instance}\nvalue -> {value}")
        def __delete__(self, instance):
            print(f"delete~\nself -> {self}\ninstance -> {instance}") 


>>> class C:
        x = D()
           
>>> c = C()
>>> c.x = 250
set~
self -> <__main__.D object at 0x000001CD25872FD0>
instance -> <__main__.C object at 0x000001CD258720A0>
value -> 250
>>> c.x
get~
self -> <__main__.D object at 0x000001CD25872FD0>
instance -> <__main__.C object at 0x000001CD258720A0>
owner -> <class '__main__.C'>
>>> del c.x
delete~
self -> <__main__.D object at 0x000001CD25872FD0>
instance -> <__main__.C object at 0x000001CD258720A0>
```

self 参数对应的是描述符这个类的实例对象，也就是相当于 x 属性的值；

instance 参数对应的是被描述符拦截的属性所在的类的实例对象；

owner 参数对应的是被描述符拦截的属性所在的类。

##### 与属性相关的方法

###### 与对象的属性访问相关的 BIF

hasattr() -- 用于判断对象是否拥有某个属性。

getarrt() -- 用于返回对象中指定属性的值。

setattr()-- 用于设置对象中指定属性的值。

delattr() -- 于删除对象中指定属性的值。

```python
>>> class C:
        def __init__(self, name, age):
            self.name = name
            self.__age = age
        
>>> c = C("小甲鱼", 18)
>>> hasattr(c, "name")
True
>>> getattr(c, "name")
'小甲鱼'
>>> getattr(c, "_C__age")
18
>>> setattr(c, "_C__age", 19)
>>> getattr(c, "_C__age")
19
>>> delattr(c, "_C__age")
>>> hasattr(c, "_C__age")
False
```

###### 与对象的属性访问相关的方法

getattr()函数对应的是 \_\_getattribute__() 这个方法

```python
>>> class C:
        def __init__(self, name, age):
            self.name = name
            self.__age = age
        def __getattribute__(self, attrname):
            print("拿来吧你~")
            return super().__getattribute__(attrname)
              
>>> c = C("小甲鱼", 18)
>>> getattr(c, "name")
拿来吧你~
>>> c._C__age
拿来吧你~
18
>>> c.FishC
拿来吧你~
Traceback (most recent call last):
  File "<pyshell#28>", line 1, in <module>
    c.y
  File "<pyshell#23>", line 7, in __getattribute__
    return super().__getattribute__(name)
AttributeError: 'C' object has no attribute 'FishC'
```

尽管最后这个 “FishC” 是一个不存在的属性，但 __getattribute__() 方法还是会作出响应，然后抛出异常

\_\_getattr__() 跟 getattr()函数长得像

也是跟访问属性有关系，不过它是只有在用户试图获取一个不存在的属性时才会被触发

```python
>>> class C:
        def __init__(self, name, age):
            self.name = name
            self.__age = age
        def __getattribute__(self, attrname):
            print("拿来吧你~")
            return super().__getattribute__(attrname)
        def __getattr__(self, attrname):
            if attrname == "FishC":
                print("I love FishC.")
            else:
                raise AttributeError(attrname)
           
>>> c = C("小甲鱼", 18)
>>> c.FishC
拿来吧你~
I love FishC.
>>> c.x
拿来吧你~
Traceback (most recent call last):
  File "<pyshell#39>", line 1, in <module>
    c.x
  File "<pyshell#35>", line 12, in __getattr__
    raise AttributeError(attrname)
AttributeError: x
```

\_\_setattr__()属性赋值(注意：容易出现问题)

```python
>>> class D:
        def __setattr__(self, name, value):
            self.name = value
        
>>> d = D()
>>> d.name = "小甲鱼"
Traceback (most recent call last):
  File "<pyshell#8>", line 1, in <module>
    d.name = "小甲鱼"
  File "<pyshell#6>", line 3, in __setattr__
    self.name = value
  File "<pyshell#6>", line 3, in __setattr__
    self.name = value
  File "<pyshell#6>", line 3, in __setattr__
    self.name = value
  [Previous line repeated 1022 more times]
RecursionError: maximum recursion depth exceeded
```

捕获到赋值操作的时候，执行 self.name = value，相当于又给自己调用一次赋值操作，那么又会触发 \_\_setattr__() 的这个方法，然后又再次执行 self.name = value 的赋值操作……  

这样就导致无限递归了

遇到这个情况有两种解决方案：  

1. 交给 super()  

2. 通过字典来存放对象属性  

对象的属性和值本来就是存放在一个叫 \_\_dict__ 的字典中：

```python
>>> class D:
        def __setattr__(self, name, value):
            self.__dict__[name] = value
        
>>> d = D()
>>> d.name = "小甲鱼"
>>> d.name
'小甲鱼'
```

\_\_delattr__()，注意无限递归，利用这个字典来实现属性的删除

```python
>>> class D:
        def __setattr__(self, name, value):
            self.__dict__[name] = value
        def __delattr__(self, name):
            del self.__dict__[name]
        
>>> d = D()
>>> d.name = "小甲鱼"
>>> d.__dict__
{'name': '小甲鱼'}
>>> del d.name
>>> d.__dict__
{}
```

##### 索引、切片、迭代协议

当对象被作为索引值使用的时候，才会调用 \_\_index__()

对象被索引的时候会去调用\_\_getitem__()

\_\_getitem__()既能响应单个下标的索引操作，又能支持代表范围的切片索引方式

```python
>>> class C:
        def __getitem__(self, index):
            print(index)
        
>>> c = C()
>>> c[2]
2
>>> c[2:8]
slice(2, 8, None) #BIF函数，切片就是它的语法糖
>>> s = "I love FishC"
>>> s[2:6]
'love'
>>> s[slice(2, 6)]
'love'
>>> s[7:]
'FishC'
>>> s[slice(7, None)]
'FishC'
>>> s[::4]
'Ivi'
>>> s[slice(None, None, 4)]
'Ivi'
```

为索引或切片赋值的操作，会被 \_\_setitem__() 方法所拦截

```python
>>> class D:
        def __init__(self, data):
            self.data = data
        def __getitem__(self, index):
            return self.data[index]
        def __setitem__(self, index, value):
            self.data[index] = value
        
>>> d = D([1, 2, 3, 4, 5])
>>> d[1]
2
>>> d[1] = 1
>>> d[1]
1
>>> d[2:4] = [2, 3]
>>> d[:]
[1, 1, 2, 3, 5]
```

\_\_getitem__() 不仅仅在拦截了索引或切片的操作，  

其实与 “获取” 相关的操作也会被拦截，比如说在 for 循环语句中：

```python
>>> class D:
        def __init__(self, data):
            self.data = data
        def __getitem__(self, index):
            return self.data[index] * 2
        
>>> d = D([1, 1, 2, 3, 5])
>>> for i in d:
        print(i)
        
2
2
4
6
10
```

###### 针对可迭代对象设计的方法

\_\_iter__() 和 \_\_next__() 是专门针对可迭代对象设计的方法 

它们对应的 BIF 函数就是 iter()和next()

如果一个对象定义了 \_\_iter__() 方法，那么它就是一个可迭代对象；  

如果一个可迭代对象定义了 \_\_next__() 方法，那么它就是一个迭代器。

比如说，列表就是一个可迭代对象，但它并不是一个迭代器

###### for 语句的工作原理

for 语句做的第一步操作，必然是先将对象传入内置函数 [iter()中，并由此拿到一个相应的迭代器。  

因为只有拿到这个迭代器，才能拥有所需的 \_\_next__() 方法。  

第二步就是利用 \_\_next__() 方法进行真正的迭代操作。

##### 代偿

**成员关系检测**

\_\_contains__() 方法用于实现成员关系的检测，它对应的运算符就是 in 和 not in

```python
>>> class C:
        def __init__(self, data):
            self.data = data
        def __contains__(self, item):
            print("嗨~")
            return item in self.data
           
>>> c = C([1, 2, 3, 4, 5])
>>> 3 in c
嗨~
True
>>> 6 in c
嗨~
False
```

###### “代偿” 原理

Python 在找不到 \_\_iter__() 和 \_\_next__() 方法的情况下，  

就会尝试去查找这个 \_\_getitem__()

\_\_contains__() 方法，也有代偿 

如果没有实现这个方法，但又使用了 in 和 not in 进行成员关系判断，  那么 Python 就会尝试去查找 \_\_iter__() 和 \_\_next__() 方法

```python
>>> class C:
        def __init__(self, data):
            self.data = data
        def __iter__(self):
            print("Iter", end=" -> ")
            self.i = 0
            return self
        def __next__(self):
            print("Next", end=" -> ")
            if self.i == len(self.data):
                raise StopIteration
            item =  self.data[self.i]
            self.i += 1
            return item
        
>>> c = C([1, 2, 3, 4, 5])
>>> 3 in c
Iter -> Next -> Next -> Next -> True
>>> 6 in c
Iter -> Next -> Next -> Next -> Next -> Next -> Next -> False
```

如果连 \_\_iter__() 和 \_\_next__() 方法都没有，那么 Python 还会试图再找找\_\_getitem__() 方法有没有定义

```python
>>> class C:
        def __init__(self, data):
            self.data = data
        def __getitem__(self, index):
            print("Getitem", end=" -> ")
            return self.data[index]
        
>>> c = C([1, 2, 3, 4, 5])
>>> 3 in c
Getitem -> Getitem -> Getitem -> True
>>> 6 in c
Getitem -> Getitem -> Getitem -> Getitem -> Getitem -> Getitem -> False
```

###### 布尔测试

如果遇到 bool()，  那么 Python 会先去寻找 \_\_bool__() 方法的定义，如果没有定义 \_\_bool__()，那么 Python 会去找找是否存在 \_\_len__() 这个方法的定义，如果有的话，返回值为非 0 就是 True，否则就是 False

###### 比较运算

\_\_lt__(self, other)：此方法返回一个布尔值，表示 “小于” 比较的结果。当使用 < 操作符比较两个对象时，会调用此方法。

\_\_le__(self, other)：此方法返回一个布尔值，表示 “小于或等于” 比较的结果。当使用 <= 操作符比较两个对象时，会调用此方法。

\_\_gt__(self, other)：此方法返回一个布尔值，表示 “大于” 比较的结果。当使用 > 操作符比较两个对象时，会调用此方法。

\_\_ge__(self, other)：此方法返回一个布尔值，表示 “大于或等于” 比较的结果。当使用 >= 操作符比较两个对象时，会调用此方法。

\_\_eq__(self, other)：此方法返回一个布尔值，表示 “等于” 比较的结果。当使用 == 操作符比较两个对象时，会调用此方法。

\_\_ne__(self, other)：此方法返回一个布尔值，表示 “不等于” 比较的结果。当使用 != 操作符比较两个对象时，会调用此方法。

###### 让方法失效

可以将方法设置为 None 来阻止它的默认行为。  

这将导致当该方法被调用时抛出一个 TypeError

```python
>>> class S(str):
        def __lt__(self, other):
            return len(self) < len(other)
        def __gt__(self, other):
            return len(self) > len(other)
        def __eq__(self, other):
            return len(self) == len(other)
        __le__ = None
        __ge__ = None
        __ne__ = None
           
>>> s1 = S("FishC")
>>> s2 = S("fishc")
>>> s1 != s2
Traceback (most recent call last):
  File "<pyshell#121>", line 1, in <module>
    s1 != s2
TypeError: 'NoneType' object is not callable
```

##### 对象调用的方法

###### 可调用性

对象的可调用性意味着该对象可以像函数一样被调用

通过定义对象的一个特殊方法 \_\_call__() 实现的

一旦 \_\_call__() 被定义，这个对象就可以像函数一样被调用

```python
>>> class C:
        def __call__(self):
            print("嗨~")
        
>>> c = C()
>>> c()
嗨~ 

#支持参数和关键字传递
>>> class C:
        def __call__(self, *args, **kwargs):
            print(f"位置参数 -> {args}\n关键字参数 -> {kwargs}")
                   
>>> c = C()
>>> c(1, 2, 3, x = 250, y = 520)
位置参数 -> (1, 2, 3)
关键字参数 -> {'x': 250, 'y': 520}
```

###### 通过定义对象的可调用性，实现类似闭包的效果

###### 对象的字符串形式

str()创建一个对象的可读性强的字符串表示。

这个表示通常是给终端用户看的，因此它的目标是可读性。当打印一个对象或将其转换为字符串以供显示时，应该使用 str()。  

repr()创建一个对象的 “正式” 字符串表示。

这个表示应该包含对象的所有重要信息，并且理想情况下，应该能够用 eval()来重新创建原始对象。这是给开发者看的，因此它的目标是无歧义和完整性。在 Python 解释器中直接输入一个对象时，Python 会使用 repr()来显示结果。

如果传入的参数是一个字符串参数的话，repr()要比 str()多了一层引号

由于 repr()的目标是生成可以用来重新创建原始对象的 Python 表达式，所以它返回的字符串表示形式含有引号

```python
>>> str(123)
'123'
>>> repr(123)
'123' 
>>> str("FishC")
'FishC'
>>> repr("FishC")
"'FishC'"
```

###### 可以用来重新创建原始对象的 Python 表达式

eval()是 Python 的内置函数，它可以解析并执行一个 Python 表达式

返回一个可以用来重新创建原始对象的 Python 表达式（以字符串的形式）

```python
>>> eval("1 + 2")
3 
>>> eval(str("FishC"))  #当变量名来引用
Traceback (most recent call last):
  File "<pyshell#0>", line 1, in <module>
    eval(str("FishC"))
  File "<string>", line 1, in <module>
NameError: name 'FishC' is not defined 
>>> eval(repr("FishC"))
'FishC'
```

在某种程度上 eval() 是 repr()的反函数

将 repr()返回的字符串，作为参数传递给 eval()，所得到的结果必定是 repr()的参数

###### 对象的字符串形式对应的方法

str()对应的方法是 \_\_str__()，repr()对应的方法是 \_\_repr__()。  

\_\_repr__() 方法是可以对 \_\_str__() 进行 “代偿” 的，也就是只定义 \_\_repr__() 方法，调用 str()也能够响应到，但反过来就不行了

 \_\_str__() 方法定义的，只能应用于对象出现在打印操作的顶层，  

比如说\把多个对象放到一个列表中，然后使用迭代进行打印，就不行了

但定义 \_\_repr__() 则没有这个问题

\_\_repr__() 的适用场景会更多，用起来也更稳

##### property()

内置函数，它用于在新式类中创建属性

将类的方法（函数）当作属性一样进行访问，这种方法称为属性装饰器

使用 property()可以在不改变类接口的情况下，增加对成员的访问控制

```python
>>> class C:
        def __init__(self):
            self._x = 250
        # 定义三个函数，分别用于获取、设置和删除 _x 属性
        def getx(self):
            return self._x
        def setx(self, value):
            self._x = value
        def delx(self):
            del self._x
        # 将上面三个函数作为参数传递给 property()，并拿到它的返回值
        x = property(getx, setx, delx, "我是 x 属性")
        
>>> c = C()
# 现在，这个没有下横线的 x 就全权代理了这个 _x
# 通过对这个 x 的访问和修改，都会影响到 _x 的值
>>> c.x
250
>>> c.x = 520
>>> c.__dict__
{'_x': 520}
>>> del c.x
>>> c.__dict__
{}
```

通过 \_\_getattr__()、\_\_setattr__() 和 \_\_delattr__() 这三个方法，也可以实现同样的效果

使用 property()的好处是：  

可读性: 使用 property()定义的属性，其行为就像普通的属性一样，可以提高代码的可读性和易用性。

封装性: property()使得可以在不暴露内部实现的情况下，对外提供属性的访问。这意味着可以在不更改 API 的情况下，改变属性背后的实现逻辑。

维护性: 当想要为单个属性添加额外的逻辑时（比如类型检查、验证等），使用 property()可以使得这些逻辑集中在一处，便于维护。

###### 将 property() 作为装饰器来使用

让创建只读属性的工作变得极为简单

```python
>>> class E:
        def __init__(self):
            self._x = 250
        @property
        def x(self):
            return self._x
        @x.setter
        def x(self, value):
            self._x = value
        @x.deleter
        def x(self):
            del self._x
                   
>>> e = E()
>>> e.x
250
>>> e.x = 520
>>> e.__dict__
{'_x': 520}
>>> del e.x
>>> e.__dict__
{}
```
