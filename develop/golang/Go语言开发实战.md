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

闭包有益于模块化编程，便于以简单的方式开发较小的模块，从而提高开发速度和程序的可复用性。和没有使用闭包的程序相比，使用闭包可将模块划分得更小。比如要计算一个数组中所有数字的和，只需要循环遍历数组，把遍历到的数字加起来就行了。如果现在要计算所有元素的积，又或者要打印所有的元素呢？解决这些问题都要对数组进行遍历，如果是在不支持闭包的语言中，程序员不得不一次又一次重复地写循环语句。而这在支持闭包的语言中是不必要的。这种处理方法多少有点像回调函数，不过要比回调函数写法更简单，功能更强大。

没有使用闭包进行计数的代码：
```
package main

import (
	"fmt"
)

func main() {
	for i := 0; i < 5; i++ {
		fmt.Printf("i=%d \t", i)
		fmt.Println(add(i))
	}
}

func add(x int) int {
	sum := 0
	sum += x
	return sum
}
```

使用闭包函数实现计数器：
```
package main

import (
	"fmt"
)

func main() {
	pos := add()
	for i := 0; i < 10; i++ {
		fmt.Printf("i=%d \t", i)
		fmt.Println(pos(i))
	}
	fmt.Println("------------")
	for i := 0; i < 10; i++ {
		fmt.Printf("i=%d \n", i)
		fmt.Println(pos(i))
	}
}

func add() func(int) int {
	sum := 0
	return func(x int) int {
		fmt.Printf("sum1=%d \t", sum)
		sum += x
		fmt.Printf("sum2=%d \t", sum)
		return sum
	}
}
```

### 可变参数
如果一个函数的参数，类型一致，但个数不定，可以使用函数的可变参数

当要传递若干个值到可变参数函数中时，可以手动书写每个参数，也可以将一个slice传递给该函数，通过“...”可以将slice中的参数对应地传递给函数

```
package main

import (
	"fmt"
)

func main() {
	sum, avg, count := GetScore(90, 82.5, 73, 93.6)
	fmt.Printf("%d-%.2f-%.2f", count, sum, avg)
	fmt.Println()

	scores := []float64{92, 65.45, 78, 98.5, 89}
	sum, avg, count = GetScore(scores...)
	fmt.Printf("%d-%.2f-%.2f", count, sum, avg)
}

func GetScore(scores ...float64) (sum, avg float64, count int) {
	for _, value := range scores {
		sum += value
		count++
	}
	avg = sum / float64(count)
	return
}
```

 - 一个函数最多只能有一个可变参数
 - 若参数列表中还有其他类型参数，则可变参数写在所有参数的最后


### 递归函数
在函数内部，可以调用其他函数。如果一个函数在内部调用自身，那么这个函数就是递归函数。递归函数必须满足以下两个条件：
1. 在每一次调用自己时，必须是在某种意义上更接近于解。
2. 必须有一个终止处理或计算的准则。

使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层，每当函数返回，栈就会减一层。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。

- 使用递归函数的优点是逻辑简单清晰，缺点是过深的调用会导致栈溢出


## 指针
指针是存储另一个变量的内存地址的变量。变量是一种使用方便的占位符，变量都指向计算机的内存地址。一个指针变量可以指向任何一个值的内存地址

在Go语言中使用取地址符 `＆` 来获取变量的地址，一个变量前使用 `＆` ，会返回该变量的内存地址

```
package main

import (
	"fmt"
)

func main() {
	a := 10
	b := &a
	fmt.Println(a, b)
}
```

**Go语言指针的最大特点是：指针不能运算（不同于C语言）**

### 声明指针
`*`号用于指定变量是一个指针
```
var a *int
var b *float32
```

指针使用流程如下:
- 定义指针变量
- 为指针变量赋值
- 访问指针变量中指向地址的值
  
获取指针指向的变量值,在指针类型的变量前加上 `*` 号，如 `*a`

```
package main

import (
	"fmt"
)

func main() {
	a := 10
	b := &a
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(*b)
}
```

### 空指针
在Go语言中，当一个指针被定义后没有分配到任何变量时，它的值为nil。nil指针也称为空指针。nil在概念上和其他语言的null、None、NULL一样，都指代零值或空值

### 通过指针修改变量的值
```
func main() {
	a := 10
	fmt.Println(a)
	b := &a
	*b++
	fmt.Println(a)
}
```

### 使用指针作为函数的参数
```
package main

import (
	"fmt"
)

func main() {
	a := 5
	var b *int = &a
	change(b)
	fmt.Println(a)
}

func change(val *int) {
	*val = 15
}
```

**将基本数据类型的指针作为函数的参数，可以实现对传入数据的修改，这是因为指针作为函数的参数只是复制了一个指针，指针指向的内存没有发生改变**


### 指针数组
元素为指针类型的数组

有一个元素个数与之相同的数组，将该数组中每个元素的地址赋值给该指针数组。也就是说该指针数组与某一个数组完全对应。可以通过 `*` 指针变量获取到该地址所对应的数值

```
package main

import "fmt"

func main() {
	a := [4]string{"abc", "xyz", "lss", "go"}
	var ptr [4]*string

	for i := 0; i < 4; i++ {
		ptr[i] = &a[i]
	}

	for _, v := range ptr {
		fmt.Println(v)
		fmt.Println()
		fmt.Println(*v)
	}
}
```

### 指针的指针
如果一个指针变量存放的又是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量。当定义一个指向指针的指针变量时，第一个指针存放第二个指针的地址，第二个指针存放变量的地址

访问指向指针的指针变量值需要使用两个 `*` 号

```
package main

import "fmt"

func main() {
	var a int = 30
	var ptr *int
	var pptr **int

	ptr = &a
	pptr = &ptr

	fmt.Println(a)
	fmt.Println(ptr)
	fmt.Println(pptr)
}
```

## 函数的参数传递
- 值传递
- 引用传递

### 值传递
值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到原内容数据

默认情况下，Go语言使用的是值传递，即在调用过程中不会影响到原内容数据。每次调用函数，都将实参复制一份再传递到函数中

每次都复制一份，性能会下降，但是Go 语言中使用指针和值传递配合就避免了性能降低问题，也就是通过传指针参数来解决实参复制的问题

### 引用传递
引用传递是指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到原内容数据

严格来说Go语言只有值传递这一种传参方式，Go语言是没有引用传递的

Go语言中可以借助传指针来实现引用传递的效果。函数参数使用指针参数，传参的时候其实是复制一份指针参数，也就是复制了一份变量地址。函数的参数如果是指针，当函数调用时，虽然参数仍然是按复制传递的，但是此时仅仅只是复制一个指针，也就是一个内存地址，这样就不用担心实参复制造成的内存浪费、时间开销、性能降低

- 指针使得多个函数能操作同一个对象
- 传指针更轻量级（8 bytes），只需要传内存地址
- 如果参数是非指针参数，那么值传递的过程中，每次在复制上面就会花费相对较多的系统开销（内存和时间）。所以要传递大的结构体的时候，用指针是一个明智的选择
- Go语言中slice、map、chan类型的实现机制都类似指针，所以可以直接传递，而不必取地址后传递指针


Go语言中所有的传参都是值传递（传值），都是一个副本。副本的内容有的是值类型（int、string、bool、array、struct属于值类型），这样在函数中就无法修改原内容数据；有的是引用类型（pointer、 slice、map、chan属于引用类型），这样就可以修改原内容数据


## Go 数组
数组是相同类型的一组数据构成的长度固定的序列，其中数据类型包含了基本数据类型、复合数据类型和自定义类型

因为数组的内存是一段连续的存储区域，所以数组的检索速度是非常快的，但是数组也有一定的缺陷，就是定义后长度不能更改

Go语言数组声明需要指定元素类型及元素个数

```
var a = [5]int{1, 2, 3, 4, 5}
var b = [...]{3, 5, 7}
```

数组长度：`len(a)`

### 遍历数组
```
//one
for i := 0; i < len(a); i++ {
    fmt.Println(a[i])
}

//two
for _, v := range a {
    fmt.Println(a)
}
```

### 多维数组

Go语言中的数组并非引用类型，而是值类型。当它们被分配给一个新变量时，会将原始数组复制出一份分配给新变量。因此对新变量进行更改，原始数组不会有反应

```
package main

import "fmt"

func main() {
	var a = [3][4]int{
		{1, 2, 3, 4},
		{10, 20, 30, 40},
		{100, 200, 300, 400},
	}
	fmt.Println(a)
}


var str [2][3]string = [2][3]string{
    {"", "", ""},
    {"", "", ""},
}
```

## Go 切片

Go语言中数组的长度不可改变，但在很多应用场景中，在初始定义数组时，数组的长度并不可预知，这样的序列集合无法满足要求

Go中提供了另外一种内置类型“切片（slice）”，弥补了数组的缺陷。切片是可变长度的序列，序列中每个元素都是相同的类型

切片的语法和数组很像。从底层来看，切片引用了数组的对象。切片可以追加元素，在追加时可能使切片的容量增大

与数组相比，切片不需要设定长度，在[]中不用设定值，相对来说比较自由

切片的数据结构可理解为一个结构体，这个结构体包含了三个元素:
- 指针，指向数组中切片指定的开始位置
- 长度，即切片的长度
- 容量，也就是切片开始位置到数组的最后位置的长度


```
slice1 := make([]string, 5)

//创建切片时可以指定容量，其中capacity为可选参数

make([]T, length,capacity)

//直接初始化
s := []int{1, 2, 3}

//通过数组截取来初始化切片
arr := [5]int{1, 2, 3, 4, 5}
s := arr[:]

//切片截取
s[1:3]
```

切片的长度是切片中元素的数量。切片的容量是从创建切片的索引开始的底层数组中元素的数量。切片可以通过`len()`方法获取长度，可以通过`cap()`方法获取容量

### 切片是引用类型
切片没有自己的任何数据。它只是底层数组的一个引用。对切片所做的任何修改都将反映在底层数组中。数组是值类型，而切片是引用类型

**修改切片数值，当多个切片共享相同的底层数组时，对每个元素所做的更改将在数组中反映出来**

- 函数append()用于往切片中追加新元素，可以向切片里面追加一个或者多个元素，也可以追加一个切片。append()会改变切片所引用的数组的内容，从而影响到引用同一数组的其他切片。当使用append()追加元素到切片时，如果容量不够（也就是(cap-len) == 0），Go就会创建一个新的内存地址来储存元素。

- 函数copy()会复制切片元素，将源切片中的元素复制到目标切片中，返回复制的元素的个数。 copy()方法不会建立源切片与目标切片之间的联系。也就是两个切片不存在联系，其中一个修改不影响另一个

## map

Go 语言提供了内置类型 map，它将一个值与一个键关联起来，可以使用相应的键检索值。这种结构在其他资料中译成地图、映射或字典，但是在Go语言中习惯上翻译成集合。map正如现实生活中的字典一样，使用词-语义进行数据的构建，其中词对应键（key），语义对应值（value），即键与值构成映射的关系，通常将两者称为键值对，这样通过键可以快速找到对应的值。map是一种集合，可以像遍历数组或切片那样去遍历它。因为map是由Hash表实现的，所以对map的读取顺序不固定

**map的value可以是任何数据类型。map和切片一样，也是一种引用类型**

## 字符串处理包
如果字符串涉及中文，遍历字符推荐使用rune。因为一个byte存不下一个汉语文字的unicode值

### strings包
### strconv包
### regexp包
### time包
### math包

## 键盘输入
### Scanln()函数
```
package main

import "fmt"

func main() {
	username := ""
	age := 0
	fmt.Scanln(&username, &age)
	fmt.Println("账号信息为:", username, age)
}
```

## Go语言面向对象编程

在Go语言中学习面向对象，主要学习结构体（struct）、方法（method）、接口（interface）

## 结构体
单一的数据类型已经满足不了现实开发需求，于是 Go 语言提供了结构体来定义复杂的数据类型。结构体是由一系列相同类型或不同类型的数据构成的数据集合

```
type People struct {
	name string
	age  int
	sex  byte
}
```

结构体的定义只是一种内存布局的描述，只有当结构体实例化时，才会真正分配内存。因此只有在定义结构体并实例化后才能使用结构体

使用内置函数`new()`对结构体进行实例化，结构体实例化后形成指针类型的结构体，`new()`内置函数会分配内存。第一个参数是类型，而不是值，返回的值是指向该类型新分配的零值的指针。该函数用于创建某个类型的指针

```
package main

import "fmt"

type People struct {
	name string
	age  int
	sex  byte
}

func main() {
	peo1 := new(People)
	fmt.Printf("peo1 : %T, %v, %p \n", peo1, peo1, peo1)
	(*peo1).name = "lss"
	(*peo1).age = 30
	(*peo1).sex = 1
	fmt.Println(peo1)

	//语法糖写法
	peo1.name = "lee ss"
	peo1.age = 25
	peo1.sex = 1
	fmt.Println(peo1)
}
```

### 结构体是值类型
结构体作为函数参数，若复制一份传递到函数中，在函数中对参数进行修改，不会影响到实际参数

值类型是深拷贝，深拷贝就是为新的对象分配了内存。引用类型是浅拷贝，浅拷贝只是复制了对象的指针

```
package main

import "fmt"

type People struct {
	name string
	age  int
	sex  byte
}

func main() {
	p1 := People{"lss", 23, '1'}
	fmt.Printf("p1 : %T, %v, %p \n", p1, p1, &p1)
	p2 := p1
	fmt.Printf("p2 : %T, %v, %p \n", p2, p2, &p2)
	p3 := &p1
	fmt.Printf("p3 : %T, %v, %p \n", p3, p3, &p3)
}
```

### 结构体做为函数的参数及返回值
结构体作为函数的参数及返回值有两种形式：值传递和引用传递

```
package main

type People struct {
	name string
	age  int
	sex  byte
}

func main() {
	p1 := People{"lss", 25, '0'}
	testfunc1(p1)
	testfunc2(&p1)
}

func testfunc1(p People) {
	//值 结构体
}

func testfunc2(p *People) {
	//指针 结构体
}
```

### 匿名结构体和匿名字段
匿名结构体就是没有名字的结构体，无须通过type关键字定义就可以直接使用。创建匿名结构体时，同时要创建对象。匿名结构体由结构体定义和键值对初始化两部分组成

```
tests := struct {
	name string
	age  int
} {
	"lss",
	25,
}
```

```
package main

import (
	"fmt"
	"math"
)

func main() {
	//匿名函数
	res := func(a, b float64) float64 {
		return math.Pow(a, b)
	}(2, 3)
	fmt.Println(res)

	//匿名结构体
	addr := struct {
		province, city string
	}{"山西省", "吕梁市"}
	fmt.Println(addr)
}
```

### 结构体嵌套
将一个结构体作为另一个结构体的属性（字段），这种结构就是结构体嵌套

```
package main

import (
	"fmt"
)

type Address struct {
	province string
	city     string
}

type Person struct {
	name    string
	age     int
	address *Address
}

func main() {
	p := Person{}
	p.name = "lss"
	p.age = 25

	addr := Address{}
	addr.province = "北京"
	addr.city = "东城"

	p.address = &addr

	fmt.Println(p)

	p.address = &Address{
		province: "sxs",
		city:     "lls",
	}

	fmt.Println(p.address)
}
```

## 方法
函数（function）是一段具有独立功能的代码，可以被反复多次调用，从而实现代码复用。而方法（method）是一个类的行为功能，只有该类的对象才能调用

**方法有接受者，而函数无接受者**

- Go语言的方法（method）是一种作用于特定类型变量的函数。这种特定类型变量叫作接受者（receiver）
- 接受者的概念类似于传统面向对象语言中的this或self关键字
- Go语言的接受者强调了方法具有作用对象，而函数没有作用对象。一个方法就是一个包含了接受者的函数。Go语言中，接受者可以是结构体，也可以是结构体类型外的其他任何类型
- 函数不可以重名，而方法可以重名


```
func (接受者变量 接受者类型) 方法名(参数列表) (返回值列表) {
	//方法体
}
```

接受者在func关键字和方法名之间编写，接受者可以是struct类型或非struct类型，可以是指针类型或非指针类型。接受者中的变量在命名时，官方建议使用接受者类型的第一个小写字母

```
package main

import (
	"fmt"
)

type Employee struct {
	name     string
	currency string
	salary   float64
}

func main() {
	emp := Employee{"lss", "###", 1000}
	emp.methodTest() //方法
	funcTest(emp)    //函数
}

//方法
func (e Employee) methodTest() {
	fmt.Println(e)
}

//函数
func funcTest(e Employee) {
	fmt.Println(e)
}
```


- Go不是一种纯粹面向对象的编程语言，它不支持类。因此其方法旨在实现类似于类的行为
- 相同名称的方法可以在不同的类型上定义，而具有相同名称的函数是不允许的
- 假设有一个正方形和一个圆形，可以分别在正方形和圆形上定义一个名为Area的求取面积的方法。

```
package main

import (
	"fmt"
	"math"
)

type Rectangle struct {
	width  float64
	height float64
}

type Circle struct {
	radius float64
}

func main() {
	r1 := Rectangle{10, 4}
	r2 := Rectangle{12, 5}
	c1 := Circle{1}
	c2 := Circle{10}

	fmt.Println(r1.Area())
	fmt.Println(r2.Area())
	fmt.Println(c1.Area())
	fmt.Println(c2.Area())
}

func (r Rectangle) Area() float64 {
	return r.width * r.height
}

func (c Circle) Area() float64 {
	return c.radius * c.radius * math.Pi
}
```

若方法的接受者不是指针，实际只是获取了一个拷贝，而不能真正改变接受者中原来的数据

方法是可以继承的，如果匿名字段实现了一个方法，那么包含这个匿名字段的struct也能调用该匿名字段中的方法

## 接口
在Go语言中，接口是一组方法签名。

接口指定了类型应该具有的方法，类型决定了如何实现这些方法。当某个类型为接口中的所有方法提供了具体的实现细节时，这个类型就被称为实现了该接口。

接口定义了一组方法，如果某个对象实现了该接口的所有方法，则此对象就实现了该接口。

Go语言的类型都是隐式实现接口的。任何定义了接口中所有方法的类型都被称为隐式地实现了该接口。


#### 接口的定义
```
package main

import "fmt"

type Phone interface {
	call()
}

type Android struct {
}

type Iphone struct {
}

func (a Android) call() {
	fmt.Println("我是安卓手机")
}

func (i Iphone) call() {
	fmt.Println("我是苹果手机")
}

func main() {
	//定义接口类型的变量
	var phone Phone

	phone = new(Android)
	fmt.Printf("%T, %v, %p \n", phone, phone, &phone)
	phone.call()
	phone = Android{}
	fmt.Printf("%T, %v, %p \n", phone, phone, &phone)
	phone.call()

	phone = new(Iphone)
	fmt.Printf("%T, %v, %p \n", phone, phone, &phone)
	phone.call()
	phone = Iphone{}
	fmt.Printf("%T, %v, %p \n", phone, phone, &phone)
	phone.call()
}
```

输出结果如下所示：
```
*main.Android, &{}, 0xc00003e1f0
我是安卓手机
main.Android, {}, 0xc00003e1f0
我是安卓手机
*main.Iphone, &{}, 0xc00003e1f0
我是苹果手机
main.Iphone, {}, 0xc00003e1f0
我是苹果手机
```

### duck typing

Go没有`implements`或`extends`关键字，这类编程语言叫作`duck typing`编程语言

duck typing是描述事物的外部行为而非内部结构。“一只鸟走起来像鸭子，游泳像鸭子，叫起来也像鸭子，那么这只鸟就可以被称为鸭子

”扩展后，可以将其理解为：“一只鸟看起来像鸭子，那么它就是鸭子。”duck typing关注的不是对象的类型本身，而是它是如何使用的

Go类型系统采取了折中的办法:
- 结构体类型T不需要显式地声明它实现了接口I。只要类型T实现了接口I规定的所有方法，它就自动地实现了接口I
- 将结构体类型的变量显式或者隐式地转换为接口I类型的变量i

```
package main

import "fmt"

type ISayHello interface {
	SayHello() string
}

type Duck struct {
	name string
}

type Person struct {
	name string
}

func (d Duck) SayHello() string {
	return d.name + "ga ga ga"
}

func (p Person) SayHello() string {
	return p.name + "Hello"
}

func main() {
	//定义实现接口的对象
	duck := Duck{"Yaya"}
	person := Person{"Jhon"}

	fmt.Println(duck.SayHello())
	fmt.Println(person.SayHello())

	fmt.Println("--------")

	//定义接口类型的变量
	var i ISayHello
	i = duck
	fmt.Printf("%T, %v, %p \n", i, i, &i)
	fmt.Println(i.SayHello())

	i = person
	fmt.Printf("%T, %v, %p \n", i, i, &i)
	fmt.Println(i.SayHello())
}
```

输出结果为：
```
Yayaga ga ga
JhonHello
--------
main.Duck, {Yaya}, 0xc00003e210
Yayaga ga ga
main.Person, {Jhon}, 0xc00003e210
JhonHello
```

### 多态
如果有几个相似而不完全相同的对象，有时人们要求在向它们发出同一个消息时，它们的反应各不相同，分别执行不同的操作。这种情况就是多态现象

多态就是事物的多种形态，Go语言中的多态性是在接口的帮助下实现的——定义接口类型，创建实现该接口的结构体对象

### 空接口
空接口中没有任何方法。任意类型都可以实现该接口。空接口这样定义：`interface{}`，也就是包含0个方法（method）的interface。空接口可表示任意数据类型

空接口常用于以下情形：
- println的参数就是空接口
- 定义一个map：key是string，value是任意数据类型
- 定义一个切片，其中存储任意类型的数据


## go 语言的异常处理
错误是指程序中出现不正常的情况，从而导致程序无法正常运行。假设尝试打开一个文件，文件系统中不存在这个文件。这是一个异常情况，它表示为一个错误

### error 接口
```
type error interface {
	Error() string
}
```

error本质上是一个接口类型，其中包含一个Error()方法，错误值可以存储在变量中，通过函数返回。它必须是函数返回的最后一个值

在Go语言中处理错误的方式通常是将返回的错误与nil进行比较。nil值表示没有发生错误，而非nil值表示出现错误。如果不是nil，需打印输出错误

### 创建 error 对象
结构体只要实现了Error() string这种格式的方法，就代表实现了该错误接口，返回值为错误的具体描述。通常程序会发生可预知的错误，所以Go语言errors包对外提供了可供用户自定义的方法，errors包下的New()函数返回error对象，errors.New()函数创建新的错误

### 自定义错误
- 定义一个结构体，表示自定义错误的类型
- 让自定义错误类型实现error接口：Error() string
- 定义一个返回error的函数。根据程序实际功能而定

```
//定义结构体，表示自定义错误的类型
type MyError struct {
	errMsg string
}

//实现 Error() 方法
func (e MyError) Error() string {
	return fmt.Printf("%s", e.errMsg)
}
```

## defer
在函数中可以添加多个defer语句。如果有很多调用defer，当函数执行到最后时，这些defer语句会按照逆序执行（报错的时候也会执行），最后该函数返回

```
package main

import (
	"fmt"
)

func main() {
	defer funcA()
	funcB()
	defer funcC()
	fmt.Println("test over ...")
}

func funcA() {
	fmt.Println("A")
}

func funcB() {
	fmt.Println("B")
}

func funcC() {
	fmt.Println("C")
}
```

输出结果为：
```
B
test over ...
C
A
```

延迟函数的参数在执行延迟语句时被执行，而不是在执行实际的函数调用时执行

```
package main

import "fmt"

func main() {
	a, b := 5, 6
	defer testDefer(a, b)

	m, n := 20, 2
	testDefer(m, n)
}

func testDefer(x, y int) {
	fmt.Println(x + y)
}
```

输出结果为：
```
22
11
```

## panic
panic，让当前的程序进入恐慌，中断程序的执行。panic()是一个内建函数，可以中断原有的控制流程

不应通过调用panic()函数来报告普通的错误，而应该只把它作为报告致命错误的一种方式。当某些不应该发生的场景发生时调用panic()

## recover
panic异常一旦被引发就会导致程序崩溃。这当然不是程序员愿意看到的，但谁也不能保证程序不会发生任何运行时错误。不过，Go语言为开发者提供了专用于“拦截”运行时panic的内建函数recover()

recover()可以让进入恐慌流程的Goroutine, 恢复过来并重新获得流程控制权。

需要注意的是，recover()让程序恢复，必须在延迟函数中执行。换言之，recover()仅在延迟函数中有效

切记，开发者应该把它作为最后的手段来使用，换言之，开发者的代码中尽量少有或者没有panic异常


## I/O 操作
计算机系统是以文件为单位来对数据进行管理的

### FileInfo 接口
文件的信息包括文件名、文件大小、修改权限、修改时间等

```
package main

import (
	"fmt"
	"os"
)

func main() {
	//绝对路径
	path := "C:/Users/T470p/Desktop/coder/jl.txt"
	fileinfo, err := os.Stat(path)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("数据类型是:%T \n", fileinfo)
		fmt.Println("文件名:", fileinfo.Name())
		fmt.Println("是否为目录:", fileinfo.IsDir())
		fmt.Println("文件大小:", fileinfo.Size())
		fmt.Println("文件权限:", fileinfo.Mode())
		fmt.Println("文件最后修改时间:", fileinfo.ModTime())
	}
}
```

输出结果为:
```
数据类型是:*os.fileStat
文件名: jl.txt
是否为目录: false
文件大小: 48
文件权限: -rw-rw-rw-
文件最后修改时间: 2020-05-10 22:59:41.4594651 +0800 CST
```

文件的权限打印出来一共10个字符。文件有3种基本权限：r（read，读权限）、w（write，写权限）、x（execute，执行权限）

### 文件路径
- filepath.IsAbs()  判断是否为绝对路径
- filepath.Rel()    获取相对路径
- filepath.Abs()    获取绝对路径
- path.Join()       拼接路径


```
package main

import (
	"fmt"
	"path"
	"path/filepath"
)

func main() {
	//绝对路径
	path1 := "C:/Users/T470p/Desktop/coder/jl.txt"
	//相对路径
	path2 := "./main.exe"

	fmt.Println(filepath.IsAbs(path1))
	fmt.Println(filepath.IsAbs(path2))

	fmt.Println(filepath.Rel("C:/Users/T470p/Desktop", path1))

	fmt.Println(path.Join(path1, ".."))
}
```

### 文件常规操作
**创建目录:**
- os.Mkdir()     创建一层目录
- os.MkdirAll()  创建多层目录


```
package main

import (
	"fmt"
	"os"
)

func main() {
	testdir1 := "./test1"
	//创建目录
	err := os.Mkdir(testdir1, os.ModePerm)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("%s 目录创建成功! \n", testdir1)
	}

	testdir2 := "./test2/abc/lss"
	//创建多级目录
	err = os.MkdirAll(testdir2, os.ModePerm)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("%s 目录创建成功! \n", testdir2)
	}
}
```

**创建文件:**
os.Create()创建文件，如果文件存在，会将其覆盖

```
package main

import (
	"fmt"
	"os"
)

func main() {
	filename1 := "./test1/abc.txt"

	//创建文件
	file1, err := os.Create(filename1)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("%s 创建成功 %v \n", filename1, file1)
	}
}
```

**打开/关闭文件**
打开文件：让当前的程序和指定的文件建立一个链接

os.Open()函数本质上是在调用os.OpenFile()函数

```
package main

import (
	"fmt"
	"os"
)

func main() {
	filename1 := "./test1/abc.txt"

	//打开文件
	file1, err := os.Open(filename1)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("%s 打开成功! %v \n", filename1, file1)
	}

	filename2 := "./test1/abs2.txt"
	//以读写的方式打开，如果文件不存在就创建
	file2, err := os.OpenFile(filename2, os.O_RDWR|os.O_CREATE, os.ModePerm)
	if err != nil {
		fmt.Println("err:", err.Error())
	} else {
		fmt.Printf("%s 打开成功! %v \n", filename2, file2)
	}

	file1.Close()
	file2.Close()
}
```

**删除文件**
- os.Remove()  删除已命名的文件或空目录
- os.RemoveAll()  删除所有的路径和它包含的任何子节点

```
package main

import (
	"fmt"
	"os"
)

func main() {
	filename1 := "./test1"

	//删除文件
	err := os.RemoveAll(filename1)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("%s 删除成功!", filename1)
	}
}
```

**读取文件**
- 打开文件 os.Open(fileName)
- 读取文件 file.Read()
- 关闭文件 file.Close()

```
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	fileName := "./jl.txt"

	//打开文件
	file, err := os.Open(fileName)
	if err != nil {
		fmt.Println("打开文件错误", err.Error())
	} else {
		bs := make([]byte, 1024*8, 1024*8)
		n := -1
		for {
			//读取文件
			n, err = file.Read(bs)
			if n == 0 || err == io.EOF {
				fmt.Println("读取文件结束")
				break
			}
			fmt.Println(string(bs[:n]))
		}
	}
	//关闭文件
	file.Close()
}
```

**写入文件**
1. 打开或创建文件  os.OpenFile()
2. 写入文件 `file.Write([]byte)-->n, err` , `file.WriteString(string)-->n, err`
3. 关闭文件 file.Close()

```
package main

import (
	"fmt"
	"os"
)

func main() {
	//打开文件
	file, err := os.OpenFile("./jl.txt", os.O_RDWR|os.O_CREATE, os.ModePerm)
	//关闭文件
	defer file.Close()

	if err != nil {
		fmt.Println("打开文件异常", err.Error())
	} else {
		n, err := file.Write([]byte("test open file"))
		if err != nil {
			fmt.Println("写入文件异常", err.Error())
		} else {
			fmt.Println("write ok", n)
		}

		n, err = file.WriteString("奥力给")
		if err != nil {
			fmt.Println("写入文件异常", err.Error())
		} else {
			fmt.Println("write ok", n)
		}
	}
}
```


**复制文件**
Go语言提供了copyFile()方法，用来复制文件


### ioutil 包

核心函数：
- ReadFile()
- WriteFile()
- ReadDir()
- TempDir()
- TempFile()


### bufio 包

#### 缓存区原理
bufio实现了带缓冲的I/O操作，达到高效读写

bufio包对io包下的对象Reader、Writer进行包装，分别实现了io.Reader和io.Writer接口，提供了数据缓冲功能，能够一定程度减少大块数据读写带来的开销，所以bufio比直接读写更快

把文件读取进缓冲区之后，再读取的时候就可以避免文件系统的输出，从而提高速度；在进行写操作时，先把文件写入缓冲区，然后由缓冲区写入文件系统

缓冲区的设计是为了存储多次的写入，最后一口气把缓冲区内容写入文件。当发起一次读写操作时，计算机会首先尝试从缓冲区获取数据；只有当缓冲区没有数据时，才会从数据源获取数据更新缓冲

实际使用中，更推荐使用Scanner对数据进行读取，而非直接使用Reader类。Scanner可以通过splitFunc将输入数据拆分为多个token，然后依次进行读取

后讲解了缓冲区的作用以及如何利用缓冲区对文件进行读写。缓冲区不仅仅用在文件的读写，在网络通信中也起到了很大的作用

## Go 语言网络编程

### HTTP
超文本传输协议（HTTP）是分布式、协作的、超媒体信息系统的应用层协议。HTTP协议在客户端-服务端架构上工作。HTTP客户端（通常为浏览器）通过URL向Web服务器发送请求。Web服务器根据接收到的请求向客户端发送响应信息。它是一个无状态的请求/响应协议

客户端请求信息和服务器响应信息都会包含请求头和请求体。HTTP请求头提供了关于发送实体的信息，如Content-Type、Content-Length、Date等。在浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header），用以响应浏览器的请求

HTTP定义了许多与服务器交互的方法，最基本的方法有4种，分别是：GET、POST、PUT、DELETE，对应着对资源的查、改、增、删4种操作。另外还有HEAD方法。HEAD类似GET方法，但只请求页面的首部，不响应页面正文部分，用于获取资源的基本信息，即检查链接的可访问性及资源是否修改

GET和POST的区别：
- GET在浏览器回退时不会再响应，而POST会再次提交请求
- GET产生的URL地址可以被添加书签，但是POST不可以
- GET请求会被浏览器主动缓存，而POST只能手动设置
- GET请求只能进行URL编码，而POST支持多种编码方式
- GET请求参数会被保存在浏览器的记录里，但POST中的参数不会被保留
- GET请求在URL中传送的参数有长度限制，而POST没有
- 对参数的数据类型，GET只接受ASCII字符，POST没有限制
- GET比POST更不安全，因为参数直接暴露在URL上，所以GET不能用来传递敏感信息
- GET参数通过URL传递，POST参数放在Request Body中


### HTTPS
安全超文本传输协议（Secure Hypertext Transfer Protocol，HTTPS）比HTTP更加安全。

HTTPS是基于SSL/TLS的HTTP，HTTP是应用层协议，TLS是传输层协议，在应用层和传输层之间，增加了一个安全套接层SSL。

服务器用RSA生成公钥和私钥，把公钥放在证书里发送给客户端，私钥自己保存。客户端首先向一个权威的服务器求证证书的合法性，如果证书合法，客户端产生一段随机数，这段随机数就作为通信的密钥，称为对称密钥。这段随机数以公钥加密，然后发送到服务器，服务器用密钥解密获取对称密钥，最后，双方以对称密钥进行加密解密通信

### HTPPS的作用
HTTPS的作用首先是内容加密，建立一个信息安全通道，来保证数据传输的安全；其次是身份认证，确认网站的真实性；最后是保证数据完整性，防止内容被第三方替换或者篡改

HTTPS和HTTP有一定的区别。HTTPS协议需要到CA申请证书。HTTP是超文本传输协议，信息是明文传输；HTTPS则是具有安全性的SSL加密传输协议。HTTP使用的是80端口，而HTTPS使用的是443端口。HTTP的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的、可进行加密传输和身份认证的网络协议，比HTTP协议安全

### HTTP协议客户端实现
Go语言标准库内置了net/http包，涵盖了HTTP客户端和服务端具体的实现方式。内置的net/http包提供了最简洁的HTTP客户端实现方式，无须借助第三方网络通信库，就可以直接使用HTTP中用得最多的GET和POST方式请求数据。

实现HTTP客户端就是客户端通过网络访问向服务端发送请求，服务端发送响应信息，并将相应信息输出到客户端的过程

```
package main

import (
	"fmt"
	"net/http"
)

func main() {
	//创建一个客户端
	client := http.Client{}

	//创建一个请求
	respone, err := client.Get("https://www.baidu.com")
	if err != nil {
		fmt.Println("error", err.Error())
	}

	if respone.StatusCode == 200 {
		fmt.Println("success")
		defer respone.Body.Close()
	}
	fmt.Println(respone)
}
```

### HTTP协议服务端实现
Go语言标准库内置的net/http包，可以实现HTTP服务端。实现HTTP服务端就是能够启动Web服务，相当于搭建起了一个Web服务器。这样客户端就可以通过网页请求来与服务器端进行交互

启动Web服务的几种方式:
1. 使用http. FileServer ()方法, http.FileServer()搭建的服务器只提供静态文件的访问。因为这种web服务只支持静态文件访问，所以称之为静态文件服务
2. 使用http. HandleFunc()方法
3. 使用http.NewServeMux()方法


## Golang 模板
模板就是在写动态页面时不变的部分，服务端程序渲染可变部分生成动态网页，Go语言提供了html/template包来支持模板渲染。Go提供的html/template包对HTML模板提供了丰富的模板语言，主要用于Web应用程序

#### 模板基本语法
待补充

## JSON 编码
JSON（JavaScript Object Notation，JavaScript对象表示法）是一种轻量级的数据交换格式，因简单、可读性强被广泛使用

#### JSON语法规则
对象是一个无序的`名称/值 对`集合。一个对象以`{`（左大括号）开始，以`}`（右大括号）结束。每个名称后跟一个`:`（冒号）, `名称/值 对`之间使用`,`（逗号）分隔。

数组是值（value）的有序集合。一个数组以`[`（左中括号）开始，以`]`（右中括号）结束。值之间使用“`,`（逗号）分隔

值（value）可以是双引号括起来的字符串（string）、数值（number）、true、false、null、对象（object）或者数组（array）。这些结构可以嵌套

字符串（string）是由双引号括起来的任意数量unicode字符的集合，使用反斜杠转义。一个字符（character）即一个单独的字符串（character string）。JSON的字符串与C或者Java的字符串非常相似

#### JSON的优点
- 数据格式比较简单，易于读写，格式都是压缩的，占用带宽小
- 便于客户端的解析，JavaScript可以轻松进行JSON数据的读取
- 支持当前主流的所有编程语言，便于服务器端的解析

Go的标准包encoding/json对JSON的支持,JSON编码即将Go数据类型转换为JSON字符串

#### map 转 json
```
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	//定义一个 map 变量并初始化
	n := map[string][]string{
		"level":   {"debug"},
		"message": {"file found", "stack over"},
	}

	//将 map 解析成 json 格式
	if data, err := json.Marshal(n); err == nil {
		fmt.Printf("%s \n", data)
	}
}
```

Marshal()函数返回的JSON字符串是没有空白字符和缩进的，这种紧凑的表示形式是最常用的传输形式，但是不好阅读。如果需要为前端生成便于阅读的格式，可以调用json.MarshalIndent()

```
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	//定义一个 map 变量并初始化
	n := map[string][]string{
		"level":   {"debug"},
		"message": {"file found", "stack over"},
	}

	//将 map 解析成 json 格式
	if data, err := json.MarshalIndent(n, "", "    "); err == nil {
		fmt.Printf("%s \n", data)
	}
}
```

输出结果为：
```
{
    "level": [
        "debug"
    ],
    "message": [
        "file found",
        "stack over"
    ]
}
```

#### 结构体转JSON

结构体转换成JSON在开发中经常会用到。json包是通过反射机制来实现编解码的，因此结构体必须导出所转换的字段，没有导出的字段不会被json包解析

```
package main

import (
	"encoding/json"
	"fmt"
)

type DebugInfo struct {
	Level  string
	Msg    string
	author string //未导出字段不会被json解析
}

func main() {
	//定义一个结构体切片并初始化
	dbgInfs := []DebugInfo{
		DebugInfo{"debig", "ok", "lss"},
		DebugInfo{"debug", "ok ok", "lsss"},
	}

	//将结构体 解析成 json 格式
	if data, err := json.Marshal(dbgInfs); err == nil {
		fmt.Printf("%s \n", data)
	}
}
```

`[{"Level":"debig","Msg":"ok"},{"Level":"debug","Msg":"ok ok"}]`

#### 结构体字段标签
json包在解析结构体时，如果遇到key为JSON的字段标签，则会按照一定规则解析该标签：第一个出现的是字段在JSON串中使用的名字，之后为其他选项。如果整个value为“-”，则不解析该字段

```
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name    string `json:"_name"`
	Age     int    `json:"_age"`
	Sex     uint   `json:"-"` //不解析
	Address string //不改变 key 标签
}

var user = User{
	Name:    "Lss",
	Age:     25,
	Sex:     1,
	Address: "深圳",
}

func main() {
	arr, _ := json.Marshal(user)
	fmt.Println(string(arr))
}
```

输出结果为：
`{"_name":"Lss","_age":25,"Address":"深圳"}`

#### 匿名字段
json包在解析匿名字段时，会将匿名字段的字段当成该结构体的字段处理

```
package main

import (
	"encoding/json"
	"fmt"
)

type Point struct{ X, Y int }
type Circle struct {
	Point
	Radius int
}

func main() {
	//解析匿名字段
	if data, err := json.Marshal(Circle{Point{50, 50}, 25}); err == nil {
		fmt.Printf("%s \n", data)
	}
}
```

输出结果为：`{"X":50,"Y":50,"Radius":25}`

- JSON对象只支持string作为key，所以要编码一个map，必须是`map[string]T`这种类型（T是Go语言中的任意类型）
- channel、complex和function是不能被编码成JSON的
- 指针在编码的时候会输出指针指向的内容，而空指针会输出nil

#### JSON转切片
