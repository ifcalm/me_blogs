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

