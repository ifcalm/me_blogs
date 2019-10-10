# Python 语法教程

### Python 的数据类型
* 整型
* 浮点型
* 字符串
* 布尔值
* 空值

布尔值：True, False 运算符号有 and, or, not

### Python 输出
`print` 语句可以向屏幕输出指定的内容

### 注释
Python 使用 `#` 进行注释

### 变量
变量必须是大小写英文字母, 数字, 下划线的组合，且不能用数字开头
```
a = 123
print a
```
这种变量本身类型不固定的语言称之为 动态语言

### 字符串
使用单引号 或 双引号 括起来

字符串用 `\` 进行转义

### 运算
* `+` 加
* `-` 减
* `*` 乘
* `/` 除
* `%` 取余

整数的运算结果永远是准确的，而浮点数的运算结果不一定精确，因为计算机内存在大，也无法精确表示出无限循环小数

### 布尔类型
* True
* False

支持的运算有 `and`, `or`, `not`，在应用的时候需要注意 **短路运算**

### List(列表)
list 是一种有序集合，可以随时添加和删除其中的元素

list 中的元素是按照顺序排列的

```
L = ['a', 'b', 'c']

直接用 [] 把元素括起来就好
```

由于 python 是动态语言，所以 list 中的元素并不要求都必须是同一种数据类型，我们完全可以在 list 中包含各种数据。

#### 按照索引访问 list
```
L = ['Adam', 'Lisa', 'Bart']
print L[0]
```

#### 可以倒序访问 list
```
print L[-1]
```

#### list 添加新元素
```
L = ['Adam', 'Lisa', 'Bart']
L.append('Lss')
```
append() 总是把新的元素添加到 list 尾部
insert() 方法接收两个参数，第一个是参数的索引号，第二个是待添加的新元素

```
L = ['Adam', 'Lisa', 'Bart']
L.insert(0, 'Lss')

将 'Lss' 添加到索引为 0 的位置，原来索引为 0 和后面的元素都自动向后移动一位
```

#### list 删除元素
`L.pop()` 删除 list 中最后一个元素，并且返回删除的这个元素

`L.pop(2)` 删除索引为 2 的元素

#### list 元素替换

```
L[2] = 'Lss'
```

完整示例：

```
L = ['Adam', 'Lisa', 'Bart']
L.append('Lss')    #添加元素
L.insert(0, 'Lqq')    #添加元素
print(L)

L2 = [0,1,2,3,4,5]
L2.pop()    #删除元素
print(L2)    #删除元素
L3 = L2.pop(2)    #返回删除的元素
print(L3)
print(L2)
```

### tuple(元组)
tuple 是另一种有序的列表（元组）

tuple 一旦创建完毕，就不能修改了

```
t = ('Adam', 'Lisa', 'Bart')

此时 t 不能更改

没有删除，插入，更改 的操作
```
单元素的 tuple 要多加一个逗号
```
t = (1,)
```

#### 注意
```
t = ('a', 'b', ['A', 'B'])
list 中的值可变
```

### dict(字典)
```
d = {
    'Adam': 95,
    'Lisa': 85,
    'Bart': 59
}

print(d)
print(d['Bart'])
```

#### 判断 key 是否存在
```
d = {
    'Adam': 95,
    'Lisa': 85,
    'Bart': 59
}

if 'Paul' in d:
    print(d['Paul'])


使用 in 操作符判断 key 是否存在
```
`in` 操作符

dict 查找速度快，无论 dict 是 10 个元素还是 10 万个元素查找速度都一样，而 list 的查找速度随着元素增加而逐渐下降

dict 占用内存大

dict 是按照 key 查找，所以在一个 dict 中，key 不能重复

dict 是无序的

dict 作为 key 的元素必须不可变

#### 遍历 dict
由于 dict 是一个集合，可以通过 for 循环实现
```
d = {''Adam': 95, 'Lisa': 85, 'Bart': 75}

for key in d:
    print(key)
```
通过 key 可以获取对应的 value

### set(集合)
dict 的作用是建立一组 key 和 一组 value 的映射关系。
dict 的key 是不能重复的。

set 的元素没有重复而且是无序的

创建set 的方式是调用 set() 并传入一个 list，list的元素将作为 set 的元素

```
s = set(['A', 'B', 'C'])

set 内部存储是无序的
```

因为 set 存储是不能包含重复的元素，所以当传入重复的元素时，set 会自动去重

#### 访问 set
set 是无序集合，所以我们没法通过索引来访问
```
s = set(['A', 'B', 'C'])
'Bart' in s
使用 in操作符 判断元素是否在set 中
```

#### 遍历 set
set 也是一个集合，可以通过 for 循环实现

```
s = set(['A', 'B', 'C'])
for str in s:
    print(str)
```

#### 添加 set 元素

```
s = set(['A', 'B', 'C'])
s.add('D')
print(s)
```

#### 删除 set 元素
```
s = set(['A', 'B', 'C'])
s.remove('A')
print(s)
```

### if 语句
```
if age >= 18:
    print('anult')
elif age >= 6:
    print('teenager')
elif age >= 3:
    print('kid')
else:
    print('baby')
```
`if` 后接 表达式，用 `:` 表示代码块开始, 在代码书写中要注意缩进, 多个条件判断时用 `if-elif-else`

### for 语句
for 可以循环任何一个数据集合
```
L = ['Adam', 'Lisa', 'Bart']
for name in L:
    print(name)
```

### while 语句
while 循环不会迭代 集合，而是根据表达式判断循环是否结束
```
N = 10
x = 0
while x < N :
    print(x)
    x = x + 1
```

### 退出循环
用 for 循环 或者 while循环时，如果在循环体内直接 退出循环，可以使用 break 语句

continue 可以跳过 后续循环代码，继续下一次循环

### 多重循环
```
for x in ['A', 'B', 'C']:
    for y in ['1', '2', '3']
        print(x + y)
```

### 内置函数
要调用一个函数，需要知道函数的名称和参数。

可以在交互式命令行中通过 `help(abs)` 来查看 `abs` 函数的帮助信息


### 自定义函数
在 python 中，定义一个函数要使用 `def` 语句，函数的返回值 用 `return` 语句

```
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x

print(my_abs(5))
```

### 函数返回多个值
```
def move():
    return 1, 2, 3

print(move())
```
函数返回多值其实就是返回一个 tuple

### 递归函数
如果一个函数在内部调用自身本身，这个函数就是递归函数

使用递归函数需要注意防止栈溢出，函数调用是通过栈这种数据结构实现的

如果递归调用的次数过多，会导致栈溢出

### 什么是堆栈溢出
堆栈溢出的产生是由于过多的函数调用，导致调用堆栈无法容纳这些调用的返回地址

### 函数的默认参数
由于函数的参数是从左到右的顺序匹配，所以默认参数只能定义在必需参数的后面
```
def test(a, b=1, c=2):
    pass
```
需要的时候又可以传入新的参数来覆盖默认的参数

### 函数之可变参数
可变参数的名字前面有个 `*` 号，我们可以传入 0个，1个 或 多个参数
```
def test(*args):
    print(args)
```
python 会把传入的一组参数装成一个 tuple 传递给可变参数。

### list 切片
```
L = [1,2,3,4,5,6,7]
L[0:3]  #取前三个元素，包括索引0，但不包括索引3
L[0:0:2] #第三个参数表示没 n 个取一个

L[-1] 取倒数第一个元素

倒序切片包含起始索引，不包含结束索引
```

### range()
range() 函数可以创建一个数列，`range(1,100)`


### 字符串切片
```
'abcd'[:1]
```

## 迭代
给定一个集合，我们可以通过 for 循环遍历这个集合，这种遍历我们称为 迭代

在 python 中，迭代是通过 `for...in...` 完成的

python 中的for 循环可以作用在任何可迭代的对象上

在 python 中，迭代永远是取出元素本身，而非元素的索引

### 列表生成式
```
L = []
for x in range(1,11):
    L.append(x*x)
```
列表生成式可以用一行语句代替循环生成上面的list：
`[x*x for x in range(1,11)]`
写列表生成式时，把要生成的元素 x*x 放在前面，后面跟 for 循环

#### 列表生成式的条件过滤
`[x*x for x in range(1,11)] if x%2 == 0`
只把偶数的平方加到列表中

#### 多层表达式
`[m+n for m in 'ABC' for n in '123']`


### 函数式编程
函数式是一种编程范式，是一种抽象计算的编程模式

函数 不等于 函数式，就像 计算 不等于 计算机
```
python ---> 函数式
c ---------> 函数
汇编语言
计算机硬件 ---> 指令
```

### 函数式编程的特点
1. 把计算视为函数而非指令
2. 纯函数式编程不需要变量
3. 支持高阶函数

### python 的函数式编程
1. 不是纯函数式编程，允许有变量
2. 支持高阶函数
3. 支持闭包
4. 支持匿名函数

### 高阶函数
所谓高阶函数就是 能接收函数作为参数的函数

1. 变量可以指向函数
2. 函数名其实就是指向函数的变量

```
def f(x):
    rturn x*x
print(map(f,[1,2,3,4]))
```
map 是内置的高阶函数，它接收一个函数和一个list，并通过把函数f 依次作用在list 的每个元素上，得到一个新的list 并返回。

### 返回函数
函数还可以返回函数

定义一个 f(),我们让他返回一个g()
```
def f():
    print('call')
    def g():
        print('pass')
    return g
```
我们在函数 f 内部又定义了一个函数 g，由于函数 g 也是一个对象，函数名g 就是指向函数g 的变量
```
x = f() ----> 调用f()
变量x 是f() 返回的函数
x() ----> x 指向函数，可以指向g() 函数定义的代码
```

注意区分返回函数和返回值：
```
def myabs():
    return abs    #返回函数

def myabs(x):
    return abs(x)   #返回函数调用的结果，返回值是一个数值
```


### 闭包
在函数内部定义函数

内层函数引用了外层函数的变量（参数也算变量），然后返回内层函数的情况，称为闭包。看如下示例：
```
def cacl_sum(lst):
    def lazy_sum():
        return sum(lst)
    return lazy_sum
```
闭包的特点是 返回函数还引用了外层函数的局部变量

### 匿名函数
`map(lambda x: x*x, [1,2,3,4])`
匿名函数：`lambda x: x*x`

匿名函数有个限制，就是只能有一个表达式，不写 return，返回值就是该表达式的结果

### 装饰器
通过高阶函数返回新函数

### 导入模块
```
import math
from math import pow,sin,log  #从math包中导入3个函数

from logging import log as loger  #为导入的函数起别名
```

### 错误和异常处理
```
while True:
    try:
        x = int(input("Please enter a number:"))
        break
    except ValueError:
        print("Try again...")
```
try 语句按如下方式工作。

首先，执行 try 子句 （在 try 和 except 关键字之间的部分）。

如果没有异常发生， except 子句 在 try 语句执行完毕后就被忽略了。

如果在 try 子句执行过程中发生了异常，那么该子句其余的部分就会被忽略。

如果异常匹配于 except 关键字后面指定的异常类型，就执行对应的except子句。然后继续执行 try 语句之后的代码。

如果发生了一个异常，在 except 子句中没有与之匹配的分支，它就会传递到上一级 try 语句中。

如果最终仍找不到对应的处理语句，它就成为一个 未处理异常，终止程序运行，显示提示信息。

一个 try 语句可能包含多个 except 子句，分别指定处理不同的异常。至多只会有一个分支被执行。异常处理程序只会处理对应的 try 子句中发生的异常，在同一个 try 语句中，其他子句中发生的异常则不作处理。一个 except 子句可以在括号中列出多个异常的名字，例如:
```
 except (RuntimeError, TypeError, NameError):
     pass
```

最后一个 except 子句可以省略异常名称，以作为通配符使用。你需要慎用此法，因为它会轻易隐藏一个实际的程序错误
```
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error: {0}".format(err))
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexpected error:", sys.exc_info()[0])
    raise
```

### 面向对象编程
