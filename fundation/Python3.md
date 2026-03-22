#### 函数

构建：

```python
def 函数名(参数1,参数2, .....):   # 参数可有可无，无需注明类型
    pass           
    #函数体，可增加return 返回对应内容，无需指定返回类型
    # 若无return， 函数默认会返回None
```

使用return 返回值，无需指定返回类型

##### 参数

###### 位置参数

实参按照形参顺序进行传递，固定位置

###### 关键字参数：

使用关键字参数，只需要知道形参的名字就可以

也可与位置参数混用，但位置参数必须在关键字参数之前

```python
>>> def myfunc(s, vt, o):
...    return "".join((o, vt, s))
...
>>> myfunc("我", "打了", "小甲鱼") #位置参数
'小甲鱼打了我'
>>> myfunc(o="我", vt="打了", s="小甲鱼")  #关键字参数
'我打了小甲鱼'打了小甲鱼'
```

###### 默认参数

在函数定义时指定值，在函数调用时，若没传入实参，采用默认值

若指定了参数值，默认值会被覆盖

如果要使用默认参数，那么应该把它们在函数定义时摆在最后

###### 只能使用位置参数：

在使用 help() 函数查看函数文档的时候，经常会在函数原型的参数中发现一个斜杠（/）

斜杠左侧的参数必须传递位置参数，不能是关键字参数，右侧随意

```python
#如：
abs(x, /)
sum(iterable, /, start=0)
```

###### 只能使用关键字参数：

星号（*）右边必须时关键字参数

```python
#如：
def abc(a, *, b, c)  #a可位置参数也可关键字参数
#b, c必须关键字参数
```

###### 收集参数

用于：传入的参数的个数是不确定的

用法：指定一个参数，然后允许调用函数时传入任意数量的参数

形参前面加上星号（*）

```python
def myfunc(*args):
    print("有%d个参数。" % len(args))
    print("第2个参数是：%s" % args[1])
```

如果在收集参数后面还需要指定其它参数，那么在调用函数的时候就应该使用关键参数来指定后面的参数

```python
>>> def myfunc(*args, a, b):
        print(args, a, b)

>>> myfunc(1, 2, 3, a=4, b=5)
(1, 2, 3) 4 5
```

除了可以将多个参数打包为元组，收集参数其实还可以将参数们打包为字典，使用连续的两个星号（**）

在传递参数的时候就必须要使用关键字参数，因为字典的元素都是键值对

等号（=）左侧是键，右侧是值

```python
>>> def myfunc(**kwargs):
        print(kwargs)


>>> myfunc(a=1, b=2, c=3)
{'a': 1, 'b': 2, 'c': 3}
```

可以混合使用：

```python
>>> def myfunc(a, *b, **c):
        print(a, b, c)

>>> myfunc(1, 2, 3, 4, x=5, y=6)
1 (2, 3, 4) {'x': 5, 'y': 6}
```

###### 解包参数

星号（*）和两个星号（**）不仅可以用在函数定义的时候，在函数调用的时候也有特殊效果

在形参上使用称之为参数的打包，在实参上的使用，则起到了相反的效果，即解包参数

```python
>>> args = (1, 2, 3, 4)
>>> def myfunc(a, b, c, d):
        print(a, b, c, d)

>>> myfunc(*args)
1 2 3 4 

>>> args = {'a':1, 'b':2, 'c':3, 'd':4}
>>> myfunc(**args)
1 2 3 4
```

###### 嵌套作用域的特性：

外层函数的作用域是会通过某种形式保存下来的，它并不会跟局部作用域那样，调用完就消失。

```python
>>> def funA():
        x = 520
        def funB():
            print(x)
        return funB
>>> funA()
<function funA.<locals>.funB at 0x0000014C02684040>
>>> funA()()
520
>>> funny = funA()
>>> funny
<function funA.<locals>.funB at 0x0000014C02684550>
>>> funny()
520
```

###### 闭包

也称工厂函数（factory function）

```python
>>> def power(exp):
        def exp_of(base):
            return base ** exp
        return exp_of

>>> square = power(2)
>>> cube = power(3)
>>> square
<function power.<locals>.exp_of at 0x000001CF6A1FAF70>
>>> square(2)
4
>>> square(5)
25
>>> cube(2)
8
>>> cube(5)
125
```

power() 函数就像是一个工厂，由于参数不同，得到了两个不同的 “生产线”

square() 返回参数的平方

cube() 返回参数的立方

##### 装饰器：

本质也是一个函数

可以让其他函数在不需要做任何代码变动的前提下增加额外的功能

```python
import time

def time_master(func):
    def call_func():
        print("开始运行程序...")
        start = time.time()
        func()
        stop = time.time()
        print("结束程序运行...")
        print(f"一共耗费了 {(stop-start):.2f} 秒。")
    return call_func

@time_master
def myfunc():
    time.sleep(2)
    print("I love Python.")

myfunc()

#运行结果：
开始运行程序...
I love Python.
结束程序运行...
一共耗费了 2.01 秒
```

使用了装饰器，并不需要修改原来的代码

只需要在函数的上方加上一个 @time_master

函数就能够实现统计运行时间的功能了

@ 加上装饰器名字其实是个语法糖

装饰器原本的样子：

```python
import time

def time_master(func):
    def call_func():
        print("开始运行程序...")
        start = time.time()
        func()
        stop = time.time()
        print("结束程序运行...")
        print(f"一共耗费了 {(stop-start):.2f} 秒。")
    return call_func

def myfunc():
    time.sleep(2)
    print("I love Python.")

myfunc = time_master(myfunc)
myfunc()
```

也可以多个装饰器：

```python
def add(func):
    def inner():
        x = func()
        return x + 1
    return inner

def cube(func):
    def inner():
        x = func()
        return x * x * x
    return inner

def square(func):
    def inner():
        x = func()
        return x * x
    return inner

@add
@cube
@square
def test():
    return 2

print(test()) 

#运行结果：65
```

运行：从下向上，先square，再cube，后add

可以给装饰器加参数：

增加一层嵌套函数来传递参数

```python
import time

def logger(msg):
    def time_master(func):
        def call_func():
            start = time.time()
            func()
            stop = time.time()
            print(f"[{msg}]一共耗费了 {(stop-start):.2f}")
        return call_func
    return time_master

@logger(msg="A")   #可以传参
def funA():
    time.sleep(1)
    print("正在调用funA...")

@logger(msg="B")  #可以传参
def funB():
    time.sleep(1)
    print("正在调用funB...")

funA()
funB() 

#运行结果：
正在调用funA...
[A]一共耗费了 1.02
正在调用funB...
[B]一共耗费了 1.01
```

装饰器原本的样子：

```python
import time

def logger(msg):
    def time_master(func):
        def call_func():
            start = time.time()
            func()
            stop = time.time()
            print(f"[{msg}]一共耗费了 {(stop-start):.2f}")
        return call_func
    return time_master

def funA():
    time.sleep(1)
    print("正在调用funA...")

def funB():
    time.sleep(1)
    print("正在调用funB...")

funA = logger(msg="A")(funA)
funB = logger(msg="B")(funB)

funA()
funB()
```

logger(msg="A") 得到的是 timemaster() 函数的引用，然后再调用一次，并传入 funA，也就是这个 logger(msg="A")(funA)，得到的就是 call_func() 函数的引用，最后将它赋值回 funA()。

其实就是多一层嵌套函数，通过最外层的这个函数来传递装饰器的参数

##### lambda 表达式

语法：lambda arg1, arg2, arg3, ... argN : expression

arg是传入的参数，expression 是函数实现表达式以及返回值

```python
#精简？？？有点像三目运算符版函数

#传统：
def squareX(x):
    return x * x
squareX(3)
#精简：
squareY = lambda y : y * y
squareY(3)
```

传统：函数名就是一个函数的引用

lambda 表达式：整个表达式就是一个函数的引用

###### lambda 表达式优势：

是一个表达式，可以用在常规函数不可能存在的地方

```python
>>> y = [lambda x : x * x, 2, 3]
>>> y[0](y[1])
4
>>> y[0](y[2])
9
```

###### 与 map() 和 filter() 函数搭配使用

```python
>>> list(mapped = map(lambda x : ord(x) + 10, "FishC"))
[80, 115, 125, 114, 77]
>>> list(filter(lambda x : x % 2, range(10)))
[1, 3, 5, 7, 9]
```

##### 生成器：

生成器是一个返回生成器对象的函数，它只能用于进行迭代操作

就是一个特殊的迭代器

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 yield 方法时从当前位置继续运行

```python
>>> def counter():
        i = 0
        while i <= 5:
            yield i
            i += 1 
>>> counter()  #返回生成器对象
<generator object counter at 0x0000025835D0D5F0> 
>>> for i in counter():
        print(i)

0
1
2
3
4
5
```

注意：

生成器不像列表、元组这些可迭代对象

可以把生成器看作是一个制作机器，它的作用就是每调用一次提供一个数据，并且会记住当时的状态。

而列表、元组这些可迭代对象是容器，它们里面存放着早已准备好的数据。

可以看作是一种特殊的迭代器， “不走回头路”，支持 next() 函数

```python
>>> c = counter()
>>> next(c)
0
>>> next(c)
1
>>> next(c)
2
>>> next(c)
3
>>> next(c)
4
>>> next(c)
5
next(c)   #没有就抛出异常
Traceback (most recent call last):
  File "<pyshell#51>", line 1, in <module>
    next(c)
StopIteration
```

生成器每调用一次获取一个结果，导致生成器对象无法使用下标索引这样的随机访问方式

生成器实现斐波那契数列：永恒的斐波那契数列生成器

```python
>>> def fib():
        back1, back2 = 0, 1
        while True:
            yield back1
            back1, back2 = back2, back1 + back2

>>> f = fib()
>>> next(f)
0
>>> next(f)
1
>>> next(f)
1
```

由于我们在函数中没有设置结束条件，只要调用 next(f)，就可以继续生成一个新的斐波那契数

###### 生成器表达式

列表有推导式，元组则没有，非要写，就成了生成器

```python
>>> t = (i ** 2 for i in range(10))
>>> next(t)
0
>>> next(t)
1
>>> next(t)
4
>>> next(t)
9
>>> next(t)
16
>>> for i in t:
        print(i)

25
36
49
64
81
```

##### 递归：

原理同c++

##### 函数文档：

help() 查看函数的使用文档，如help(abs)

创建函数文档，使用字符串，函数文档一定是在函数的最顶部

函数开头的几行字符串并不会被打印出来，但它将作为函数的文档被保存起来

```python
>>> def exchange(dollar, rate=6.32):
        """
        功能：汇率转换，美元 -> 人民币
        参数：
        - dollar 美元数量
        - rate 汇率，默认值 6.32（2022-03-08）
        返回值：
        - 人民币数量
        """
        return dollar * rate

>>> exchange(20)
126.4
#通过help也可查到
```

##### 类型注释：

只是函数作者的寄望（给人看的，不是给机器看的），如不按要求传入，python也不会阻止

希望调用者传入到 s 参数的是字符串类型，传入到 n 参数的是整数类型，最后还告诉我们函数将会返回一个字符串类型的返回值

```python
>>> def times(s:str, n:int) -> str:
        return s * n
```

##### 内省

通过一些特殊的属性来实现内省

使用 __name__查看一个函数的名字

使用 __*annotations_\* 查看函数的类型注释

使用 __doc__查看函数文档

##### 高阶函数

一个函数接收另一个函数作为参数

模块：functools

###### reduce()

将可迭代对象中的元素依次传递到第一个参数指定的函数中，最终返回累积的结果

第一个参数是指定一个函数，这个函数必须接收两个参数，然后第二个参数是一个可迭代对象

```python
>>> def add(x, y):
        return x + y

>>> functools.reduce(add, [1, 2, 3, 4, 5])
15
```

第一个参数也可写成 lambda 表达式

###### 偏函数

对指定函数的二次包装，通常是将现有函数的部分参数预先绑定，从而得到一个新的函数

原理：闭包

```python
>>> square = functools.partial(pow, exp=2)
>>> square(2)
4
>>> square(3)
9
>>> cube = functools.partial(pow, exp=3)
>>> cube(2)
8
>>> cu
be(3)
27
#原理大致为：
def partial(func, /, *args, **keywords):
    def newfunc(*fargs, **fkeywords):
        newkeywords = {**keywords, **fkeywords}
        return func(*args, *fargs, **newkeywords)
    newfunc.func = func
    newfunc.args = args
    newfunc.keywords = keywords
    return newfunc
```

###### @wraps 装饰器

装饰装饰器

```python
import time
import functools

def time_master(func):
    @functools.wraps(func)
    def call_func():
        print("开始运行程序...")
        start = time.time()
        func()
        stop = time.time()
        print("结束程序运行...")
        print(f"一共耗费了 {(stop-start):.2f} 秒。")
    return call_func

@time_master
def myfunc():
    time.sleep(2)
    print("I love FishC.")

myfunc()  
#运行：
开始运行程序...
I love FishC.
结束程序运行...
一共耗费了 2.01 秒
>>> myfunc.__name__
'myfunc'

#之前装饰器的名字：
>>> myfunc.__name__
'call_func'
```

#### 永久存储：

##### 打开文件：

open(文件名，打开方式)

返回一个文件对象f

可以使用文件对象提供的方法

写：

f.write() 返回值总共写入到文件对象中的字符个数

f.writelines() 多个字符串同时写入，需要自己加换行符

##### 关闭文件：

f.close()

##### 路径：

使用 Path 里面的 cwd() 方法来获取当前的工作目录

如：Path.cwd()

创建路径对象：

p = Path('C:/Users/goodb/AppData/Local/Programs/Python/Python39')

支持索引

使用斜杠（/）直接进行路径拼接

q = p / "test.txt"

q.is_dir() 判断一个路径是否为一个文件夹

q.is_file() 判断一个路径是否为一个文件

q.exists() 测试指定的路径是否真实存在

q. name 属性去获取路径的最后一个部分

q.stem 属性用于获取文件名

q.suffix 属性用于获取文件后缀

q.parent 属性用于获取其父级目录

q.parts 属性将路径的各个组件拆分成元组

q.stat() 查询文件或文件夹的状态信息

q.stat().st_size 就是文件或文件夹的尺寸信息

iterdir() 获取当前路径下面的所有子文件和子文件夹迭代器对象

mkdir() 来创建文件夹

rename()文件或文件夹修改名字

rmdir() 删除文件夹，不空删不掉

unlink() 删除文件

glob() 查找文件，如"*.txt"

##### with 语句和上下文管理器

打开文件 -> 操作文件 -> 关闭文件

```python
>>> with open("test.txt", "w") as f:
        f.write("I love Python.")
```

###### pickle 模块

将源代码转变成 0101001 的二进制组合(序列化)

dump() 将数据写入文件中（文件后缀要求是 .pkl）

load() 读取 pickle 文件中的数据

```python
pickle.dump((x, y, z, s, l, d), f)
x, y, z, s, l, d = pickle.load(f)
le.dump(d, f)
```

#### 异常：

```python
try:
    检测范围
except [expression [as identifier]]:
    异常处理代码
[except [expression [as identifier]]:
    异常处理代码]
[else:
    没有触发异常时执行的代码]
[finally:
    收尾工作执行的代码]

#或
try:
    检测范围
finally:
    收尾工作执行的代码
```

###### try-except

捕获并处理异常

```python
try:
    检测范围
except [expression [as identifier]]:
    异常处理代码
```

可以将多个可能出现的异常使用元组的形式给包裹起来

```python
>>> try:
        1 / 0
        520 + "FishC"
    except (ZeroDivisionError, ValueError, TypeError):
        pass

#或
>>> try:
        1 / 0
        520 + "FishC"
    except ZeroDivisionError:
        print("除数不能为0。")
    except ValueError:
        print("值不正确。")
    except TypeError:
        print("类型不正确。")
```

###### try-except-else

当 try 语句没有检测到任何异常的情况下，就执行 else 语句的内容

###### try-except-finally

无论异常发生与否，都必须要执行的语句

finally通常是用于执行那些收尾工作，比如关闭文件的操作

###### 异常的嵌套

```python
>>> try:
        try:
            520 + "FishC"
        except:
            print("内部异常！")
        1 / 0
    except:
        print("外部异常！")
    finally:
        print("收尾工作~")

内部异常！
外部异常！
收尾工作~
```

###### raise 语句

手动的引发异常，只能raise一个存在的异常

raise ValueError("值不正确。")

##### assert语句

主动引发异常，用于代码调试
