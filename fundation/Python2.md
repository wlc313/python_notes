#### 列表：

使用[ ]包起来

##### 基本操作

```python
# 索引访问
l[0]      # 第一个元素
l[-1]     # 最后一个元素
l[1:3]    # 切片 范围：[1, 3)

# 添加元素
l.append(x)       # 末尾添加
l.extend(iter)    # 扩展可迭代对象
l.insert(i, x)    # 在索引i处插入x

# 删除元素
l.remove(x)       # 删除第一个匹配项
l.pop([i])        # 弹出索引i（默认末尾）
l.clear()         # 清空列表

# 排序
l.sort()          # 原地排序
l.reverse()       # 原地反转
```

##### 添加：

处理：添加 append(一个) 或 extend(一堆)

##### 删除：

remove(内容)

若有多个相同内容，只会删除第一个

若元素不存在，抛出异常

##### 查：

count(内容) 返回内容出现次数

index(内容) 返回内容第一次出现的索引值

index(内容，start，end)  根据位置查找内容，并返回第一个找到的索引值

##### 复制：

浅拷贝：只拷贝外层对象

两种方式：copy()或[:]

深拷贝：

import copy

y = copy.deepcopy(x)

字符串是不可变的

列表默认是可变的

*是重复引用

##### 列表推导式：效率比for循环高，基本等同于c++

[**expression** for target in iterable]

结果一定是列表

每个for取到的元素，进行expression的操作

二维数组同理

进阶：[expression for target in iterable **if condition**]

增加筛选条件（主要针对target）

顺序：先执行if 再执行 for 最后expression

再进阶：嵌套

[**expression** for target1 in iterable1 if condition1

                       for target2 in iterable2 if condition2

                        .....

                        for targetN in iterableN if conditionN]

```python
# 如：
# 列表推导式
[i**2 for i in range(10)]

# 带条件
[i for i in range(10) if i % 2 == 0]

# 嵌套
[[j for j in range(3)] for i in range(3)]
```

#### 元组：

在（）包起来

（）也可以省略

元素的内容是不可变的，不能修改

但可以使用索引访问

```python
# 创建
t = (1, 2, 3)
t = 1, 2, 3        # 可省略括号

# 打包与解包
a = 1, 2, 3        # 打包
x, y, z = a        # 解包

# 单元素元组
t = (1,)           # 注意逗号
```

也支持切片、支持查操作、支持嵌套、支持“+”和“*”的使用

没有元组推导式，但可以将元组元素在列表推导式中使用

生成一个元素的元组，元素后需要加“，”，否则是int类型

#### 打包和解包：

适用于元组，列表，字符串

注意：等号左边的变量个数，必须与右边的元素个数一致，否则报错

元组里可以装列表元素，这样的列表元素是可以修改的

---

#### 字符串：

使用“”包起来

##### 大小写转换：

capitalize()  首字母大写，其余字母小写，返回新串，不改原串

caseflod()  返回小写（和lower不同，可多种语言）

title()  返回每个单词首字母大写的串

swapcase() 返回大小写反转的串

upper()  所有大写

lower()  所有小写

##### 左中右对齐：给的宽度大于原字符串宽度才会对齐

width： 对齐宽度，fillchar：填充字符，默认为空格

center(width, fillchar=' ')  居中

ljust(width, fillchar=' ') 左对齐

rjust(width, fillchar=' ')  右对齐

zfill(width)  0填充左侧

##### 查找：

count(sub[,start[,end]])    查找内容出现的次数，起始~终止下标-1

定位下标值，找不到为-1：

find(sub[,start[,end]])  从左往右找

rfind(sub[,start[,end]]) 从右往左找

定位下标值，找不到抛出异常

index(sub)

##### 替换：

expandtabs([tabsize=8])  使用空格替换制表符，返回一个新的字符串

replace(old,new,count=-1)  old的子字符串替换成new的新字符串 count设置替换次数，默认替换全部

translate(table) 先使用sstr.maketrans(x[,y[,z]])获取表格

table为转换规则

sstr.maketrans(x[,y[,z]]) 第三个参数是表示忽略什么字符串

##### 判断：都返回bool类型

startswith(prefix[,start[,end]]) 判断子字符串是否是在字符串的**起始**位置

endswith(prefix[,start[,end]]) 判断子字符串是否是在字符串的**结束**位置

上面两个可以支持元组查找

isupper() 判断是否都是大写

islower() 判断是否都是小写

istitle() 判断每个单词的首个字母是否大写

isalpha() 判断是否全部是由字母构成（有空格也会false）

isascii()

isspace() 是否空白字符串（所有不可见字符，包括'\n'都是true）

isprintable() 判断是否都是可打印字符（'\n'不可打印）

检测是否是数字：

isdecimal()

isdigit()

isnumeric()

isalnum() 若isalpha(),isdecimal(),isdigit(),isnumeric()有任一个true，结果为true

isidentifier() 判断是否是一个合法的标识符

补充一点其他知识：判断是否是保留标识符：

import keyword

keyword.iskeyword("想要查询能否使用的标识符")

##### 截取：

strip(chars=None) 去除所有留白

lstrip(chars=None)  去除左侧留白

rstrip(chars=None)  去除右侧留白

上面三个函数，若无参数，默认去除空白

若有参数，则去除对应位置的对应字符（按单个字符为单位剔除，直到遇到没有对应的字符）

removeprefix(prefix)  去除前缀，指定子字符串

removesuffix(suffix) 去除后缀，执行子字符串

##### 拆分

以指定元素进行拆分，结果存入三元组

partition(sep) 从左到右找分隔符

rpartition(sep)  从右到左找分隔符

split(sep=None, maxsplit=-1)  从左向右找sep进行分割

按sep分割，分割maxsplit份

若无参数，默认切分空格

rsplit(sep=None, maxsplit=-1) 从右向左找sep进行分割

splitlines() 按行分割

参数可以加true，分割的元素中会包含换行符

由于各系统换行符不同，不用"\r","\n"分割

##### 拼接：

join(iterable)

用法：分隔符.join(列表或元组)

将列表或元组中的内容用分隔符链接拼成整串

join效率比使用”+“拼接效率好，由其数量很大时

##### 格式化字符串：

使用{}表示替换字段（占位置）

真的内容放在后面的format(内容)中

{}中也可以写上数字，表示用第几个参数，使用下标索引表示

索引值可以多次引用

{}中可以写关键字，通过关键字索引

若想输出花括号，如下两种：

```python
"{},{},{}".format(1,"{}",2)
"{},{{}},{}".format(1,2)
```

{}内可添加格式：

[[fill]align] [sign] [#] [0] [width] [grouping_option] [.precision][type]

常用：

索引:填充对齐内容 对齐方式 0填充 宽度

[sign] 只对数字有效，值”+“，”-“，” “

[grouping_option] 只对数字有效，每三位添加一个分隔符

[.precision] 精度：

当[type] 类型为f或F，限定小数点**后**显示多少位

当[type] 类型为g或G，限定小数点**前后**一共显示多少位

对非数字类型，限定最大字段长度

对于整数，不允许使用[.precision]选项

[type] : 类型

##### f-字符串（py3.6以上版本）

字符串前面加f或F，与上面使用的{}规则相同

不用写format，可以在{}内直接写变量名或运算规则等内容

----

#### 序列：

可变序列：列表

不可变序列：元组、字符串

当进行+或*运算时，可变序列编号不变，不可变序列编号改变

id() 查看编号

is 和is not 同一性运算符

in和not in 判断包含和不包含问题

del 用于删除一个或多个指定的对象

del 是“斩草除根”，若要实现clear的效果（清空列表），需要用切片的方法

##### 可使用的相关函数：

**列表、元组、字符串相互转换：**

list() 转换为列表

tuple() 转换为元组

str() 转换为字符串

**找可迭代对象中的最小最大值（字符看ascii）**

min(iterable, *[,key,default])

min(arg1,arg2,*args[,key])

max(iterable, *[,key,default])

max(arg1,arg2,*args[,key])

**检测长度：**

len()  长度太大，会报错

（小等c++中基本类型可存储的长度正常，否则报错）

**计算求和：**

sum()  可以设置start初始值

**排序：**

sorted() 排序，并返回一个新的列表，不改变原列表，括号里支持任何形式的可迭代对象

sort() 排序，改变原列表，只能是列表.sort()

**翻转**

reversed() 翻转，返回的是一个参数的反向迭代器（可看作可迭代对象），括号里支持任何形式的可迭代对象

reverse()  翻转，列表使用

**判断元素为真**

all() 判断可迭代对象中是否**所有元素**的值都为真

any() 判断可迭代对象中是否**存在某个元素**的值都为真

**构成二元组**

enumerate() 返回一个枚举对象， 将可迭代对象中的每个元素及从0开始的序号共同构成一个二元组的列表

**列表对应位置组合：**

zip() 创建一个举个多个可迭代对象的迭代器

若各个可迭代对象的长度不一致，则以最小的为准，多余的部分丢弃

若不想丢弃，则引入import itertools,使用其中的zip_longest()

**通过特定函数处理可迭代对象**

map() 根据提供的函数对指定的可迭代对象的每个元素进行运算，并返回**运算结果**的迭代器

若各个可迭代对象的长度不一致，则以最小的为准，多余的部分丢弃

filter() 根据提供的函数对指定的可迭代对象的每个元素进行运算，并将运算**结果为真**的元素，以迭代器的形式返回

##### 区别迭代器和可迭代对象

迭代器是一个可迭代对象

可迭代对象可以重复使用，而迭代器则是一次性的

关键看函数的返回值

可迭代对象变迭代器：iter() 

将迭代器中的内容逐个取出：next()

---

#### 字典：dict

使用{}包起来

一个键对应一个值，不能出现重复的键

##### 增：

fromkeys(iterable[,values]) 对初始化友好

##### 删：

pop() 抛出一个键的值，并从字典里去除

popitem() 3.7版本前，随机删除一个键值对，3.7版本后，删除最后一个加入字典的键值对

也可用del进行删除

clear()清空字典

##### 改：

 可以根据键直接更改值

update()更新多个键的值

##### 查：

可直接通过键查值，但若没有对应的键，就会报错

可使用get()查询，通过default设置没找到键的情况

可使用setdefault()，通过键查找值，若找不到，增加键及对应值

获取视图对象（下面三个，字典内容改变，视图对象也会改变）

items() 键值

keys()  键

values() 值

copy() 复制字典

len() 获得键值对数量

in 或 not in 判断某个键是否存在于字典中

list() 可以转换成列表(默认是由键构成的列表)

iter() 将字典的键构成迭代器

reversed() 翻转字典顺序（3.8版本后可用）

##### 嵌套：

值可以进行嵌套（字典、列表）

##### 字典推导式：

使用方法同列表推导式，注意键值

---

#### 集合：

使用{}包起来

特点：唯一性、无序性

集合中的所有元素都是独一无二的，也是无序的

因为无序，所以不能使用下标访问

可以使用in和not in判断某个元素是否存在于集合中

可以轻松去重

##### 常用方法：

copy()  复制

isdisjoint() 检测两个集合之间是否毫不相干

issubset() 检测集合是否是另一个的子集

issuperset() 检测集合是否是另一个集合的超集

union() 和其它集合构成并集

intersection() 交集

difference() 差集

symmetric_difference() 对称差集

也可用运算符进行操作，操作两边必须是集合

<  <=  子集

\> >= 超集

| 并集

& 交集

\- 差集

^ 对称差集

##### 集合分为可变和不可变对象

frozenset 不可变对象

set：可变对象

下面是只可set使用的方法

update() 更新集合，若不在集合中，则添加进去（迭代装入，拆开）

下面三个都会改变集合

intersection_update() 交集

difference_update()  差集

symmetric_difference_update() 对称差集

add() 添加数据，不迭代，整体

remove(elem) 删除元素，不存在会抛出异常

discard(elem) 删除元素，不存在静默处理

pop() 随机从集合中弹出一个元素

clear() 清空集合

##### 可哈希

字典的键 和 集合都要求是可哈希的

hash() 和获取一个对象的哈希值

大多不可变的对象都是哈希的，可变对象是不可哈希的

集合想要嵌套，需要使用frozenset()

特定情况下，集合的效率比列表高出很多

因为集合的背后由散列表的支持（用空间换时间）
