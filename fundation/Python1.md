# Python

### 基本概念

#### 输入输出

变量名 = input("提示语") //类型自动识别

print()

字符串单双引号都可以

字符串前加"r"，表示直接输出

一行输入未结束，末尾加""可跨行

三引号字符串是长字符串（三单或三双），可跨行

重复打印：

print("abc\n" * 100) abc字符串会重复打印100遍

#### 基本运算：

##### 随机数：

import random

random.randint(1,10) 生成1-10之间的随机数

x=random.getstate() 随机数种子

random.setstate() 用之前的随机生成数替换随机数种子序列（使用randint会重复之前的随机生成数）

##### 浮点数：

默认计算结果是浮点数，采用IEEE754，小数会出现误差

这样判断会出错（false）：if 0.1+0.2 == 0.3

精确比较采用下面方式 (true)：

```python
import decimal
a = decimal.Decimal('0.1')
b = decimal.Decimal('0.2')
c = decimal.Decimal('0.3')
if a + b == c
```

##### 虚数

x = 1 + 2j 1是实部，2是虚部

x.real 获得x的实部

x.imag 获得x的虚部

##### 运算符

算数运算符：

| `+`  | 加   | `1 + 2` → `3`   |
| ---- | --- | --------------- |
| `-`  | 减   | `3 - 1` → `2`   |
| `*`  | 乘   | `2 * 3` → `6`   |
| `/`  | 除   | `6 / 2` → `3.0` |
| `//` | 取整除 | `7 // 2` → `3`  |
| `%`  | 取余  | `7 % 2` → `1`   |
| `**` | 幂   | `2 ** 3` → `8`  |

成员运算符

```python
in       # 在...中
not in   # 不在...中
```

身份运算符

```python
is       # 是同一对象
is not   # 不是同一对象
```

”//“ 除法结果向下取整，取比目标结果小的最大整数

如 3 // 2 ->1 -3 // 2 -> -2

abs(x) 若x是整数或是浮点数，取绝对值

若是虚数，取模

complex还可以写为complex("1+2j")

**多重赋值：** x, y = 10, 20 //x = 10 y = 20

**部分函数：**

| abs(x)          | x的绝对值              |
| --------------- | ------------------ |
| int(x)          | 将x转换成整数            |
| float(x)        | 将x转换成浮点数           |
| complex(re, im) | 返回一个复数，re是实部，im是虚部 |
| c.conjugate()   | 返回c的共轭复数           |
| divmod(x, y)    | 返回 (x // y, x % y) |
| pow(x, y)       | 计算x的y次方            |

##### bool

bool(x) 其中x只有是下面情况才是假，其余都是真

> 定义为False的对象：None 和 False

> 值为0的数字类型：0, 0.0, 0j, Decimal(0), Fraction(0, 1)

> 空的序列和集合："", (), [], {}, set(), range(0)

##### 逻辑运算符

and or not

特殊部分：

and 和 or 短路逻辑：从左往右，只有第一个操作数的值无法确定逻辑运算时，才对第二个操作数进行求**值**

如 ： 3 and 4 -> 4 0 and 3 -> 0

3 or 4 -> 3 0 or 4 -> 4

##### 运算符优先级：

越小优先级越低，越大优先级越高

#### 分支循环结构：

##### 条件分支：

结构1：

if 条件 : 事情1

else：事情2

还可写为：

事情1 if 条件 else 事情2

如：

```python
a = 3
b = 5
if a < b :
    small = a
else:
    small = a
"""也可写为："""
small = a if a < b else b
print(small)
```

结构2：

if 条件 : 事情1

elif 条件：事情2

else：事情3

##### 循环结构：while

while 条件:

可以搭配else使用

（正常条件变为假，会执行else中的内容，若使用break跳出，则else内容不再会执行）

使用else可以便捷的检测到循环是否“寿终正寝”

##### 循环结构：for

格式：

> for 变量 in 可迭代对象：

按次数循环，需要搭配range()

range()用法：

1. range(end) 从0~end-1之间的数

2. range(start, end) 从start ~ end-1 之间的数

3. range(start, end, step) 从 start ~end-1之间，步长为step的数

循环遍历：

```python
for i in range(n):
    ...
for k, v in dict.items():
    ...

"""字符串："""
for each in "abc":
    print(each)
"""对比while"""
while i < len("abc")
    print("abc"[i])
    i += 1
```
