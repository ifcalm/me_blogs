## Python 入门

### Python 语言的特点

- 语法简单
- 类库丰富
- 开放源码
- 跨平台
- 可扩展

### Python 版本

Python 最流行的发行版Anaconda

- iPython
- jupyter notebook
- pip


### Python 保留字

```
import keyword
keyword.kwlist
```

保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 `keyword` 模块，可以输出当前版本的所有关键字


### 注释

Python 中单行注释以 `#` 开头

多行注释可以用多个 `#`

多行注释还可以使用 `'''` 和 `"""`

```
print("ifcalm")

'''
多行注释
'''

"""
多行注释
"""
```

### 行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号 `{}`

### 多行语句

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠`\`来实现多行语句

```
total = "item_one" + \
    "item_two" + \
    "item_three"

print(total)
```

### Python 数字类型

- int
- bool
- float
- complex


### Python 字符串

**python中单引号和双引号使用完全相同**

转义字符 `\`

反斜杠可以用来转义，使用`r`可以让反斜杠不发生转义

```
str = "if calm ok \n gmail com"
str_2 = r"if calm ok \n gmail com"

print(str)
print(str_2)
```

- 字符串可以用 `+` 运算符连接在一起
- 用 `*` 运算符重复
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始
- Python 没有单独的字符类型，一个字符就是长度为 1 的字符串
- 字符串的截取的语法格式为: `变量[头下标:尾下标:步长]`


```
str = "ifcalm.ok@gmail.com"

print(str)
print(str[0:-1])
print(str[0])
print(str[2:5])
print(str[2:])
print(str[1:8:2])        #输出从第二个开始到第五个且每隔两个的字符
print(str * 2)           #输出字符串两次
print("email: " + str)   #拼接字符串
```

### 等待用户输入

`input("请输入")`


### 同一行显示多条语句

```
import sys; x = "ifcalm"; sys.stdout.write(x + "\n")
```

Python可以在同一行中使用多条语句，语句之间使用分号`;`分割


### print 输出

`print` 默认输出是换行的, 如果要实现不换行需要在变量末尾加上 `end=""`

```
print("ifcalm", end = "")
print("gmail", end = "")
```

### import 和 from ... import

在 python 用 `import` 或者 `from...import` 来导入相应的模块

- 将整个模块导入，格式为： `import somemodule`
- 从某个模块中导入某个函数,格式为： `from somemodule import somefunction`
- 从某个模块中导入多个函数,格式为： `from somemodule import firstfunc, secondfunc, thirdfunc`
- 将某个模块中的全部函数导入，格式为： `from somemodule import *`


### 变量

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建

在 Python 中，变量就是变量，它没有类型，我们所说的类型是变量所指的内存中对象的类型

等号`=` 用来给变量赋值

### 多个变量赋值

Python允许你同时为多个变量赋值

```
# 创建一个整型对象，值为 1，从后向前赋值，三个变量被赋予相同的数值
a = b = c = 1
print(a)
print(b)
print(c)

x, y, z = 3, "ifcalm", True
print(x)
print(y)
print(z)
```

### 标准数据类型

- 数字
- 字符串
- 列表
- 元组
- 集合
- 字典

不可变数据: 数字, 字符串, 元组

可变数据: 列表, 字典, 集合(set)


### 数字类型

Python3 支持 `int、float、bool、complex（复数）`

在Python 3里，只有一种整数类型 `int`，表示为长整型

内置的 `type()` 函数可以用来查询变量所指的对象类型

```
x, y, z = 3, "ifcalm", True
print(x)
print(y)
print(z)
print(type(x))
print(type(y))
print(type(z))
```

输出的内容为:

```
3
ifcalm
True
<class 'int'>
<class 'str'>
<class 'bool'>
```

还可以用 `isinstance` 来判断类型:

```
x, y, z = 3, "ifcalm", True
v = isinstance(x, int)
print(v)
```

### isinstance 和 type 的区别

待补充


```
class A:
    pass

class B(A):
    pass

v_1 = isinstance(A(), A)
print(v_1)

if (type(A()) == A):
    print(True)
else:
    print(False)

v_2 = isinstance(B(), A)
print(v_2)

if (type(B() == A)):
    print(True)
else:
    print(False)
```


**以使用`del`语句删除一些对象引用**

```
x, y, z = 3, "ifcalm", True
del x     # 删除单个对象
del x, y  # 删除多个对象
```


### 数值运算

- `+`, 加法
- `-`, 减法
- `*`, 乘法
- `/`, 除法，得到一个浮点数
- `//`, 除法，得到一个整数
- `%`, 取余
- `**`, 乘方


![](img/py_1.PNG)

数值的除法包含两个运算符：`/` 返回一个浮点数，`//` 返回一个整数

在混合计算时，Python会把整型转换成为浮点数


### 字符串

- Python中的字符串用单引号 `'` 或双引号 `"` 括起来，同时使用反斜杠 `\` 转义特殊字符
- 字符串的截取的语法格式: `变量[头下标:尾下标]`
- 索引值以 0 为开始值，-1 为从末尾的开始位置
- 加号 `+` 是字符串的连接符， 星号 `*` 表示复制当前字符串，与之结合的数字为复制的次数


```
str = "ifcalm.ok@gmail.com"

print(str)
print(str[0:-1])
print(str[0])
print(str[2:5])
print(str[2:])
print(str * 2)
print(str + ": email")
```


Python 使用反斜杠 `\` 转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 `r`，表示原始字符串

```
str = "emial: \n ifcalm.ok@gmail.com"

print(str)
print(r'emial: \n ifcalm.ok@gmail.com')
print(r"emial: \n ifcalm.ok@gmail.com")
```

**Python 没有单独的字符类型，一个字符就是长度为1的字符串**


**与 C 字符串不同的是，Python 字符串不能被改变。向一个索引位置赋值，比如`word[0] = 'm'`会导致错误**

#### 字符串总结

- 反斜杠可以用来转义，使用`r`可以让反斜杠不发生转义
- 字符串可以用`+`运算符连接在一起，用`*`运算符重复
- Python中的字符串有两种索引方式，从左往右以0开始，从右往左以-1开始
- Python中的字符串不能改变


### 列表 --> List

列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表，所谓嵌套

列表是写在方括号 `[]` 之间、用逗号分隔开的元素列表

和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表

`list[头下标:尾下标]`，索引值以 0 为开始值，-1 为从末尾的开始位置

```
list = [1024, "ifcalm", True, 3.14, [1, 2, 3], ['a', 'b', 'c']]

print(list)
print(list[0])
print(list * 2)
print(list[1:3])
```

与Python字符串不一样的是，列表中的元素是可以改变的

```
list = [1, 2, 3, 4, 5]
list[0] = 10
print(list)
list[2:4] = []
print(list)
del list[2]  # 删除列表元素
```

List 内置了有很多方法，例如 `append()`、`pop()` 等

列表对 `+` 和 `*` 的操作符与字符串相似。`+` 号用于组合列表，`*` 号用于重复列表

#### Python列表函数

- len(list), 列表元素个数
- max(list), 返回列表元素最大值
- min(list), 返回列表元素最小值
- list(seq), 将序列转换为列表


#### Python 列表方法

- list.append(obj), 在列表末尾添加新的对象
- list.count(obj), 统计某个元素在列表中出现的次数
- list.extend(seq), 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
- list.index(obj), 从列表中找出某个值第一个匹配项的索引位置
- list.insert(index, obj), 将对象插入列表
- list.pop(index), 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值
- list.remove(obj), 移除列表中某个值的第一个匹配项
- list.reverse(), 反转列表中元素
- list.sort( key=None, reverse=False), 对原列表进行排序
- list.clear(), 清空列表
- list.copy(), 复制列表


#### list 总结:

- List写在方括号之间，元素用逗号隔开
- 和字符串一样，list可以被索引和切片
- List可以使用+操作符进行拼接
- List中的元素是可以改变的


### 元组

元组与列表类似，不同之处在于**元组的元素不能修改**。元组写在小括号 `()` 里，元素之间用逗号隔开, 元组中的元素类型也可以不相同

```
tuple = (1024, "ifcalm", 3.14, True, "python")
print(tuple)
print(tuple[0])
print(tuple[2:5])
print(tuple * 2)
del tuple  # 删除元组
```

元组与字符串类似，可以被索引且下标索引从0开始，-1 为从末尾开始的位置。也可以进行截取。其实，可以把字符串看作一种特殊的元组

**修改元组元素的操作是非法的**

**虽然tuple的元素不可改变，但它可以包含可变的对象，比如list列表**

#### 元组内置函数

- len(tuple), 计算元组元素个数
- max(tuple), 返回元组中元素最大值
- min(tuple), 返回元组中元素最小值
- tuple(seq), 将序列转换为元组

#### 元组总结:

- 与字符串一样，元组的元素不能修改
- 元组也可以被索引和切片，方法一样
- 注意构造包含 0 或 1 个元素的元组的特殊语法规则
- 元组也可以使用+操作符进行拼接


**string、list 和 tuple 都属于 sequence（序列）**

### 集合 --> Set

集合是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员

**集合的基本功能是进行成员关系测试和删除重复元素**


可以使用大括号 `{ }` 或者 `set()` 函数创建集合，注意：创建一个空集合必须用 `set()` 而不是 `{ }`，因为 `{ }` 是用来创建一个空字典

**集合是无序的**

```
s_1 = {1, 2,3, 4, 5, 6}
s_2 = set("ifcalm")
print(s_1)
print(s_2)

# 成员测试
if 4 in s_1:
    print("4 在集合中")
else:
    print("4 不在集合中")

# set 可以进行集合运算
a = set("abcdxyz")
b = set("hijgabc")
print(a - b)      # a和 b的差集
print(a | b)      # a 和b 的并集
print(a & b)      # a 与b 的交集
print(a ^ b)      # a和b 中不同时存在的元素
```


### 字典

列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取

字典是一种映射类型，字典用 `{ }` 标识，它是一个无序的 `key:value` 的集合

- 键(key)必须使用不可变类型
- 在同一个字典中，键(key)必须是唯一的


```
d_1 = {}
d_1["one"] = "python"
d_1["two"] = "golang"

print(d_1)

d_2 = {"name": "lss", "age": 25, "site": "ifcalm.net"}

print(d_2)
print(d_2.keys())   # 输出所有的键
print(d_2.values()) # 输出所有的值
del d_2["name"]     # 删除一个值
```


#### 字典类型总结

- 字典是一种映射类型，它的元素是键值对
- 字典的关键字必须为不可变类型，且不能重复
- 创建空字典使用 `{ }`

字典值可以是任何的 python 对象，既可以是标准的对象，也可以是用户定义的

### Python 数据类型转换

- int(x), 将 x 转换为一个整数
- float(x), 将 x 转换为一个浮点数
- str(x), 将对象x转换为字符串
- tuple(s), 将序列 s 转换为元组
- list(s), 将序列 s 转换为列表
- set(s), 将序列转换为集合
- dict(d), 创建一个字典


```
str_1 = "4"
val_1 = int(str_1)
print(val_1)
print(float(val_1))

str_2 = "ifcalm"
print(tuple(str_2))
print(list(str_2))
print(set(str_2))
```

### python 注释

单行注释以 `#` 开头

多行注释用三个单引号 `'''` 或者三个双引号 `"""` 将注释括起来

### Python 运算符

- 算术运算符
- 比较运算符
- 赋值运算符
- 逻辑运算符
- 位运算符
- 成员运算符
- 身份运算符


#### 算术运算符

- `+`
- `-`
- `*`
- `/`
- `%`
- `**`, 幂运算
- `//`, 除法取整运算


#### 比较运算符

比较运算符结果是 `True` 和 `False`

- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`


#### 赋值运算符

- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `%=`
- `**=`
- `//==`
- `:=`, Python3.8 版本新增的变量赋值运算符


#### 位运算符

- `&`, 与
- `|`, 或
- `^`, 异或
- `~`, 取反
- `<<`, 左移
- `>>`, 右移


#### 逻辑运算符

- `and`, 与
- `or`, 或
- `not`, 非


#### 成员运算符

- `in`, 如果在指定的序列中找到值返回 True，否则返回 False
- `not in`, 如果在指定的序列中没有找到值返回 True，否则返回 False


#### 身份运算符

- `is`, `is` 是判断两个标识符是不是引用自一个对象, `x is y`, 类似 `id(x) == id(y)` , 如果引用的是同一个对象则返回 `True`，否则返回 `False`
- `is not`, `x is not y` ， 类似 `id(a) != id(b)`。如果引用的不是同一个对象则返回结果 `True`，否则返回 `False`

**id() 函数用于获取对象内存地址**

身份运算符用于比较两个对象的存储单元


### Python 数字

python 支持三种不同的数值类型:

- int
- float
- complex

#### 数字类型转换

- `int(x)` 将x转换为一个整数
- `float(x)` 将x转换到一个浮点数
- `complex(x)` 将x转换到一个复数，实数部分为 x，虚数部分为 0
- `complex(x, y)` 将 x 和 y 转换到一个复数，实数部分为 x，虚数部分为 y。x 和 y 是数字表达式


### Python 字符串

Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用

Python 访问子字符串，可以使用方括号 `[]` 来截取字符串

在需要在字符中使用特殊字符时，python 用反斜杠 `\` 转义字符

#### Python 字符串运算符

- `+`, 字符串拼接
- `*`, 输出重复字符串
- `[]`, 通过索引获取字符串中的字符
- `[:]`, 截取字符串中的一部分, 遵循左闭右开的原则, 右边界不取
- `in`, 成员运算符, 如果字符串中包含给定的字符返回 True
- `not in`, 成员运算符, 如果字符串中不包含给定的字符返回 True
- `%`, 格式字符串


#### Python 字符串格式化

```
print("My name is %s, age is %d" %("lss", 25))
```

python字符串格式化符号:

- `%s`, 格式化字符串
- `%d`, 格式化整数
- `%f`, 格式化浮点数字


#### Python 三引号

python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其他特殊字符

```
str = """这是一个多行字符串的实例
多行字符串可以使用制表符
TAB ( \t )。
也可以使用换行符 [ \n ]。
"""
print (str)
```

三引号让程序员从引号和特殊字符串的泥潭里面解脱出来，自始至终保持一小块字符串的格式是所见即所得）格式的

一个典型的用例是，当你需要一块HTML或者SQL时，这时用字符串组合，特殊字符串转义将会非常的繁琐


#### f-string

f-string 是 python3.6 之后版本添加的，称之为字面量格式化字符串，是新的格式化字符串的语法, 之前我们习惯用百分号 `%`


f-string 格式化字符串以 `f` 开头，后面跟着字符串，字符串中的表达式用大括号 `{}` 包起来，它会将变量或表达式计算后的值替换进去

```
name = "ifcalm"
print(f"My name is {name}")
print(f"{1+5}")
```

用了这种方式明显更简单了，不用再去判断使用 `%s`，还是 `%d`


在 Python 3.8 的版本中可以使用 `=` 符号来拼接运算表达式与结果

```
x = 1
print(f"{x+1=}")
```

**在Python3中，所有的字符串都是Unicode字符串**


#### Python 的字符串内建函数

待补充


**关键字end可以用于将结果输出到同一行，或者在输出的末尾添加不同的字符**

```
a, b = 0, 1
while (b < 100):
    print(b, end=',')
    a, b = b, a+b
```

### Python 条件控制

Python 条件语句是通过一条或多条语句的执行结果（`True` 或者 `False`）来决定执行的代码块

```
if condition_1:
    pass
elif condition_2:
    pass
else:
    pass
```

Python 中用 `elif` 代替了 `else if`，所以`if`语句的关键字为：`if – elif – else`

- 每个条件后面要使用冒号 `:`，表示接下来是满足条件后要执行的语句块
- 使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块
- 在Python中没有`switch – case`语句


#### if 嵌套

在嵌套 `if` 语句中，可以把 `if...elif...else` 结构放在另外一个 `if...elif...else` 结构中

### Python 循环语句

Python 中的循环语句有 `for` 和 `while`

**在 Python 中没有 `do..while` 循环**

#### while 循环使用 else 语句

```
while expr:
    pass
else:
    pass
```

#### for 语句

Python `for`循环可以遍历任何序列的项目，如一个列表或者一个字符串

```
for var in seq:
    pass
else:
    pass
```

**break 语句用于跳出当前循环体**


#### range()函数

如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列
```
for i in range(5):
    print(i)
```

你也可以使用range指定区间的值
```
for i in range(5, 9):
    print(i)
```

也可以使range以指定数字开始并指定不同的增量(甚至可以是负数，有时这也叫做'步长')
```
for i in range(0, 10, 3):
    print(i)
```

#### 中断循环

- break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行
- continue 语句被用来告诉 Python 跳过当前循环块中的剩余语句，然后继续进行下一轮循环


### pass 语句

Python `pass`是空语句，是为了保持程序结构的完整性,`pass` 不做任何事情，一般用做占位语句


### Python 迭代器与生成器

迭代是Python最强大的功能之一，是访问集合元素的一种方式

**迭代器是一个可以记住遍历的位置的对象**

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退

迭代器有两个基本的方法:
- iter()
- next()

字符串，列表或元组对象都可用于创建迭代器:
```
list = [1, 2, 3, 4]
it = iter(list)
print(next(it))
print(next(it))
```

迭代器对象可以使用常规for语句进行遍历:
```
list = [1, 2, 3, 4]
it = iter(list)
for x in it:
    print(x, end=" ")
```

#### 生成器

在 Python 中，使用了 `yield` 的函数被称为生成器

跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器

在调用生成器运行的过程中，每次遇到 `yield` 时函数会暂停并保存当前所有的运行信息，返回 `yield` 的值, 并在下一次执行 `next()` 方法时从当前位置继续运行。

调用一个生成器函数，返回的是一个迭代器对象

```
import sys

# 生成器函数 - 斐波那契
def fib(n):
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n):
            return
        yield a
        a, b = b, a+b
        counter += 1

# f 是一个迭代器，由生成器返回生成
f = fib(10)

while True:
    try:
        print(next(f), end=" ")
    except StopIteration:
        sys.exit()
```


### Python 函数

函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段

函数能提高应用的模块性，和代码的重复利用率

#### 定义一个函数

- 函数代码块以 `def` 关键词开头，后接函数标识符名称和圆括号 `()`
- 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数
- `return [表达式]` 结束函数，选择性地返回一个值给调用方，不带表达式的 `return` 相当于返回 `None`


```
def 函数名(参数列表):
    函数体
```

```
def max(a, b):
    if a > b:
        return a
    else:
        return b

a = 4
b = 5
print(max(a, b))
```

#### 函数调用

#### 匿名函数

python 使用 `lambda` 来创建匿名函数


#### return 语句

`return [表达式]` 语句用于退出函数，选择性地向调用方返回一个表达式。不带参数值的`return`语句返回`None`


### Python 数据结构

- 列表
- 元组
- 集合
- 字典

### 列表推导式

每个列表推导式都在 `for` 之后跟一个表达式，然后有零到多个 `for` 或 `if` 子句。返回结果是一个根据表达从其后的 `for` 和 `if` 上下文环境中生成出来的列表。如果希望表达式推导出一个元组，就必须使用括号

```
list = [2, 3, 4]
l_1 = [2*x for x in list]

l_2 = [[x, x**2] for x in list]

l_3 = [3*x for x in list if x > 3]
```


### 遍历

在字典中遍历时，关键字和对应的值可以使用 `items()` 方法同时解读出来

在序列中遍历时，索引位置和对应值可以使用 `enumerate()`函数同时得到


```
list = [25, "ifcalm", 3.14, "email", True]
for i, v in enumerate(list):
    print(i, v)
```


### Python 模块

模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。这也是使用 python 标准库的方法


#### import 语句

```
import sys
```

#### from … import 语句

Python 的 `from` 语句让你从模块中导入一个指定的部分到当前命名空间中

#### from … import * 语句

把一个模块的所有内容全都导入到当前的命名空间也是可行的


### 包


### Python 输入输出

- 输出, print()
- 输入, input()


### Python 文件处理

### Python os模块


### Python 错误和异常

异常捕捉可以使用 `try/except` 语句

```
while True:
    try:
        x = int(input("请输入一个数字: "))
        break
    except ValueError:
        print("您输入的不是数字，请再次尝试输入！")
```


`try` 语句按照如下方式工作:

- 首先，执行 `try` 子句
- 如果没有异常发生，忽略 `except` 子句，`try` 子句执行后结束
- 如果在执行 `try` 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。如果异常的类型和 `except` 之后的名称相符，那么对应的 `except` 子句将被执行
- 如果一个异常没有与任何的 `except` 匹配，那么这个异常将会传递给上层的 `try` 中


一个 `try` 语句可能包含多个`except`子句，分别来处理不同的特定的异常。最多只有一个分支会被执行


一个`except`子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组

```
except (RuntimeError, TypeError, NameError):
    pass
```

最后一个`except`子句可以忽略异常的名称，它将被当作通配符使用。你可以使用这种方法打印一个错误信息，然后再次把异常抛出

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

`try/except` 语句还有一个可选的 `else` 子句，如果使用这个子句，那么必须放在所有的 `except` 子句之后

```
for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except IOError:
        print('cannot open', arg)
    else:
        print(arg, 'has', len(f.readlines()), 'lines')
        f.close()
```

`else` 子句将在 `try` 子句没有发生任何异常的时候执行


#### try-finally 语句

`try-finally` 语句无论是否发生异常都将执行最后的代码

```
try:
    ifcalm()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
finally:
    print('这句话，无论异常是否发生都会执行。')
```

#### 抛出异常

Python 使用 `raise` 语句抛出一个指定的异常

```
x = 10
if x > 5:
    raise Exception('x 不能大于 5。x 的值为: {}'.format(x))
```

### Python 面向对象

面向对象基本特征:

- 类
- 方法
- 类变量
- 对象
- 方法重写
- 局部变量
- 继承
- 实例化


类的继承机制允许多个基类，派生类可以覆盖基类中的任何方法，方法中可以调用基类中的同名方法


#### 类定义

```
class ClassName:
    pass
```

#### 类对象

```
class MyClass:
    """一个简单的类实例"""
    i = 12345
    def f(self):
        return 'hello world'
 
# 实例化类
x = MyClass()
 
# 访问类的属性和方法
print("MyClass 类的属性 i 为：", x.i)
print("MyClass 类的方法 f 输出为：", x.f())
```

