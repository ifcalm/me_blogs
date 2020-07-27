#### 变量的概念
- 变量是计算机语言中储存数据的基本单元。变量的功能是存储数据
- 变量的本质是计算机分配的一小块内存，专门用于存放指定数据，在程序运行过程中该数值可以发生改变；变量的存储往往具有瞬时性，或者说是临时存储，当程序运行结束，存放该数据的内存就会释放，该变量就会随着内存的释放而消失

#### 全局变量与局部变量
- 局部变量，是定义在大括号`（{}）`内部的变量，大括号的内部也是局部变量的作用域。
- 全局变量，是定义在函数和大括号`（{}）`外部的变量。

#### 变量声明
```
var a int

var (
    a int
    b string
    c bool
    d []float32
    e func() bool
    f struct {
        x int
        y string
    }
)
```

**未初始化变量的默认值有如下特点。**
- 整型和浮点型变量默认值：0。
- 字符串默认值为空字符串。
- 布尔型默认值为false。
- 函数、指针变量、切片默认值为nil。

**初始化变量**
```
var a int = 3
var b = 4
c := 5
```

使用 `:=` 赋值操作符可以高效地创建一个新的变量，称为初始化声明。声明语句省略了 `var` 关键字，变量类型将由编译器自动推断。这是声明变量的首选形式，但是它只能被用在`函数体内`，而不可以用于全局变量的声明与赋值。

#### 变量多重赋值
变量多重赋值是指多个变量同时赋值。Go语法中，变量初始化和变量赋值是两个不同的概念。Go语言的变量赋值与其他语言一样，但是Go提供了其他程序员期待已久的多重赋值功能，可以实现变量交换。多重赋值让Go语言比其他语言减少了代码量。

```
    var a int = 10
	var b int = 20
	var tmp int
	tmp = a
	a = b
	b = tmp

    ---------------------

    var a int = 10
    var b int = 20
    b, a = a, b
```

#### 匿名变量
Go语言的函数可以返回多个值，而事实上并不是所有的返回值都用得上。那么就可以使用匿名变量，用下画线`_`替换即可。

```
package main

import "fmt"

func main() {
	a, _ := test() //舍弃第二个值
	_, b := test() //舍弃第一个值
	fmt.Println(a, b)
}

func test() (int, int) {
	return 1, 2
}
```

**匿名变量既不占用命名空间，也不会分配内存**

#### 数据类型
在计算机中，操作的对象是数据，那么大家来思考一下，如何选择合适的容器来存放数据才不至于浪费空间？

在Go语言中，有以下几种数据类型：
- 基本数据类型：整型、浮点型、复数型、布尔型、字符串、字符（byte、rune）
- 复合数据类型：数组（array）、切片（slice）、映射（map）、函数（function）、结构体（struct）、通道（channel）、接口（interface）、指针（pointer）

#### 整形
- 有符号整型：int8、int16、int32、int64、int
- 无符号整型：uint8、uint16、uint32、uint64、uint

**其中uint8就是byte型**

#### 浮点型
浮点型表示存储的数据是实数，如3.475

- float32, 4个字节，32位的浮点型
- float64, 8个字节，64位的浮点型

```
var x float32
var y float64
```

#### 复数型
复数型用于表示数学中的复数，如`1+2j、1-2j、-1-2j`等

- complex64, 8个字节，64位的复数型
- complex128, 16个字节，128位的复数型

#### 布尔型
布尔型用预定义标识符bool表示，
布尔型的值只可以是常量true或者false

```
var flag bool
```
**布尔型无法参与数值运算，也无法与其他类型进行转换**


#### 字符串
字符串在Go语言中是以基本数据类型出现的

**定义多行字符串的方法如下：**
- 双引号书写字符串被称为字符串字面量，这种字面量不能跨行
- 多行字符串需要使用反引号 ` ，多用于内嵌源码和内嵌数据
- 在反引号中的所有代码不会被编译器识别，而只是作为字符串的一部分

```
package main

import "fmt"

func main() {
	var temp string
	temp = `
		x := 10
		y := 20
		fmt.Println(x, y)
	`
	fmt.Println(temp)
}

```

#### 字符
字符串中的每一个元素叫作字”，定义字符时使用`单引号`。Go语言的字符有两种

- byte , 1个字节，uint8的别名类型，表示 utf-8 字符串的单个字节的值
- rune , 4个字节，int32的别名类型，表示单个 unicode 字符

```
var a byte = 'a'
var b rune = '我'
```

#### 打印格式化
打印格式化通常使用fmt包

- %v, 值的默认格式表示
- %+v, 类似%v，但输出结构体时会打印字段名
- %#v，值的Go语法表示
- %T，值的类型Go语法表示

```
package main

import "fmt"

func main() {
	str := "hello, world"
	fmt.Printf("%T, %v \n", str, str)
	var a rune = '帅'
	fmt.Printf("%T, %v \n", a, a)
}
```

#### 布尔型打印格式

%t, 打印内容为 `true` 或 `false`

```
package main

import "fmt"

func main() {
	var flag bool
	fmt.Printf("%T, %t \n", flag, flag)
	flag = true
	fmt.Printf("%T, %t \n", flag, flag)
}
```

#### 整形打印格式

- %b, 表示二进制
- %d, 表示十进制
- %x, 表示十六进制

#### 浮点型与复数型的打印格式

- %b
- %e
- %E
- %f
- %F
- %g
- %G


#### 字符串打印格式

- %s, 直接输出字符串
- %q


#### 数据类型转换

Go语言采用数据类型前置加括号的方式进行类型转换，格式如：T（表达式）。T表示要转换的类型；表达式包括变量、数值、函数返回值等

**类型转换时，需要考虑两种类型之间的关系和范围，是否会发生数值截断**

**布尔型无法与其他类型进行转换**

```
var a int = 10
b := float64(a)  //将 int型转换成 float64 型
c := string(a)   //将 int型转换成 string 型
```

float和int的类型精度不同，使用时需要注意float转int时精度的损失

#### 整形转字符串类型
这种类型的转换，其实相当于byte或rune转string。

**在Go语言中，不允许字符串转int**


#### 常量
相对于变量，常量是恒定不变的值，如圆周率。常量是一个简单值的标识符，在程序运行时，不会被修改。常量中的数据类型只可以是布尔型、数字型（整型、浮点型和复数型）和字符串。

```
const A int = 5
Const S string = "TEST"
```
可以省略类型说明符，因为编译器可以根据变量的值来自动推断其类型
```
const A = 3
```

常量组中如果不指定类型和初始值，则与上一行非空常量的值相同
```
package main

import "fmt"

const (
	a = 10
	b
	c
)

func main() {

	fmt.Println(a, b, c)
}
```
输出结构都为 10


#### iota
iota，特殊常量值，是一个系统定义的可以被编译器修改的常量值。iota只能被用在常量的赋值中，在每一个const关键字出现时，被重置为0，然后每出现一个常量，iota所代表的数值会自动增加1。iota可以理解成常量组中常量的计数器，不论该常量的值是什么，只要有一个常量，那么iota就加1

```
package main

import "fmt"

const (
	a = iota
	b = iota
	c = iota
)

func main() {
	fmt.Println(a, b, c)
}
```
输出结果为 0 1 2


#### 类型别名
说到类型别名，无非是给类型名取一个有特殊含义的外号而已，对于编程而言，类型别名主要用于解决兼容性的问题。

`type AliasStr = string` 将 AliasStr 定义为 string 的一个别名，使用 AliasStr 与 string 等效。


#### 运算符
- ，+ , - , * , / , %
- ++ , --
- == , != , > , < , >= , <=
- && , || , !
- = , += , -= , *= , /= ,


#### 位运算符
位运算符对整数在内存中的二进制位进行操作。

位运算符比一般的算术运算符速度要快，而且可以实现一些算术运算符不能实现的功能。如果要开发高效率程序，位运算符是必不可少的。位运算符用来对二进制位进行操作，包括：按位与（＆）、按位或（|）、按位异或（^）、按位左移（＜＜）、按位右移（＞＞）

- &    对应的二进制位相与
- |
- ^    异或，相同为0，不同为1
- <<   左移运算符，左移n位就是乘以2的n次方
- .>>


取址运算符：&
指针变量：*

**注意运算符的优先级**

### 流程控制
- if
- if ... else..
- switch
- select
- for
- break
- continue
- goto
- 循环嵌套

**if变体**
```
package main

import "fmt"

func main() {
	if num := 10; num%2 == 0 {
		fmt.Println(num)
	} else {
		fmt.Println(num)
	}
}
```

### 函数与指针

Go 语言从设计上对函数进行了优化和改进，让函数使用起来更加方便。因为Go语言的函数本身可以作为值进行传递，既支持匿名函数和闭包，又能满足接口

### 函数作为值
在Go语言中，函数也是一种类型，可以和其他类型，如int32、float等等，一样被保存在变量中

函数作为值的使用步骤：
-  定义一个函数类型
-  实现定义的函数类型
-  作为参数调用

### 匿名函数
Go语言支持匿名函数，即在需要使用函数时再定义函数。匿名函数没有**函数名**，只有函数体，函数可以作为一种类型被赋值给变量，匿名函数也往往以变量方式被传递

匿名函数经常被用于实现回调函数、闭包等
```
//语法格式

func(a, b int) (x, y int) {
		return a, b
	}
```

### 在定义时调用匿名函数
```
package main

import "fmt"

func main() {
	func(data int) {
		fmt.Println("hello, ", data)
	}(12345)
}

```

### 将匿名函数赋值给变量
```
package main

import "fmt"

func main() {
	f := func(data string) {
		fmt.Println(data)
	}
	f("Hello Go !")
}

```

### 匿名函数作为回调函数
```
package main

import (
	"fmt"
	"math"
)

func main() {
	arr := []float64{1, 9, 16, 25, 30}
	visit(arr, func(v float64) {
		v = math.Sqrt(v)
		fmt.Printf("%.2f \n", v)
	})

	visit(arr, func(v float64) {
		v = math.Pow(v, 2)
		fmt.Printf("%.0f \n", v)
	})
}

func visit(list []float64, f func(float64)) {
	for _, value := range list {
		f(value)
	}
}
```

### 闭包
闭包是由函数和与其相关的引用环境组合而成的实体。在实现深约束时，需要创建一个能显式表示引用环境的东西，并将它与相关的子程序捆绑在一起，这样捆绑起来的整体被称为闭包。函数 + 引用环境 = 闭包

闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例

