```
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

func main() {
	fmt.Println("Please visit http://127.0.0.1:12345/")
	http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
		s := fmt.Sprintf("你好，世界！, Time: %s", time.Now().String())
		fmt.Fprintf(w, "%v \n", s)
		log.Printf("%v \n", s)
	})

	if err := http.ListenAndServe(":12345", nil); err != nil {
		log.Fatal("ListenAndServe", err)
	}
}
```

## 数组-字符串-切片
在主流的编程语言中数组及其相关的数据结构是使用得最为频繁的，只有在它不能满足时才会考虑链表、散列表和更复杂的自定义数据结构

**散列表可以看作是数组和链表的混合体**

Go语言中数组、字符串和切片三者是密切相关的数据结构。这3种数据类型，在底层原始数据有着相同的内存结构，在上层，因为语法的限制而有着不同的行为表现

Go语言的数组是一种值类型，虽然数组的元素可以被修改，但是数组本身的赋值和函数传参都是以整体复制的方式处理的

Go语言字符串底层数据也是对应的字节数组，但是字符串的只读属性禁止了在程序中对底层字节数组的元素的修改

**字符串赋值只是复制了数据地址和对应的长度，而不会导致底层数据的复制**

切片的结构和字符串结构类似，但是解除了只读限制。切片的底层数据虽然也是对应数据类型的数组，但是每个切片还有独立的长度和容量信息，切片赋值和函数传参时也是将切片头信息部分按传值方式处理

**因为切片头含有底层数据的指针，所以它的赋值也不会导致底层数据的复制**

Go语言的赋值和函数传参规则很简单，除**闭包函数以引用的方式对外部变量访问之外**，其他赋值和函数传参都是以传值的方式处理


### 数组
数组是一个由固定长度的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。数组的长度是数组类型的组成部分

因为数组的长度是数组类型的一部分，不同长度或不同类型的数据组成的数组都是不同的类型

数组对应的类型是切片，切片是可以动态增长和收缩的序列，切片的功能也更加灵活

数组定义:
```
var a [3]int
var b [...]int{1, 2, 3}
var c [...]int{2: 3, 1: 2}   //元素为 0, 2, 3 (索引2对应的值为3)
var d [...]int{1, 2, 4: 5, 6} //元素为 1, 2, 0, 0, 5, 6
```
第一种方式是定义一个数组变量的最基本的方式，数组的长度明确指定，数组中的每个元素都以零值初始化

第二种方式是定义数组，可以在定义的时候顺序指定全部元素的初始化值，数组的长度根据初始化元素的数目自动计算

第三种方式是以索引的方式来初始化数组的元素，因此元素的初始化值出现顺序比较随意。这种初始化方式和`map[int]Type`类型的初始化语法类似。数组的长度以出现的最大的索引为准，没有明确初始化的元素依然用零值初始化

第四种方式是混合了第二种和第三种的初始化方式，前面两个元素采用顺序初始化，第三个和第四个元素采用零值初始化，第五个元素通过索引初始化，最后一个元素跟在前面的第五个元素之后采用顺序初始化

Go语言中数组是值语义。一个数组变量即表示整个数组，它并不是**隐式地指向第一个元素的指针**,例如C语言的数组

当一个数组变量被赋值或者被传递的时候，实际上会复制整个数组。如果数组较大的话，数组的赋值也会有较大的开销。为了避免复制数组带来的开销，可以传递一个指向数组的指针，但是数组指针并不是数组

```
package main

import (
	"fmt"
)

func main() {
	var a = [...]int{1, 2, 3}
	var b = &a //b是指向数组的指针
	fmt.Println(a[0], a[1])
	fmt.Println(b[0], b[1])

	for i, v := range b {
		fmt.Println(i, v)
	}
}
```

其中b是指向数组a的指针，但是通过b访问数组中元素的写法和a是类似的。还可以通过for range来迭代数组指针指向的数组元素。其实数组指针类型除类型和数组不同之外，通过数组指针操作数组的方式和通过数组本身的操作类似，而且数组指针赋值时只会复制一个指针。但是数组指针类型依然不够灵活，因为数组的长度是数组类型的组成部分，指向不同长度数组的数组指针类型也是完全不同的

遍历数组的几种方式:
```
package main

import (
	"fmt"
)

func main() {
	var a = [...]int{1, 2, 3}

	for i := range a {
		fmt.Printf("a[%d]: %d\n", i, a[i])
	}

	for i, v := range a {
		fmt.Printf("a[%d]: %d\n", i, v)
	}

	for i := 0; i < len(a); i++ {
		fmt.Printf("a[%d]: %d\n", i, a[i])
	}
}
```

用for range方式迭代的性能可能会更好一些，因为这种迭代可以保证不会出现数组越界的情形，每轮迭代对数组元素的访问时可以省去对下标越界的判断

用for range方式迭代，还可以忽略迭代时的下标:
```
var times [5][0]int
for range times {
	fmt.Println("hello")
}
```
其中times对应一个[5][0]int类型的数组，虽然第一维数组有长度，但是数组的元素[0]int大小是0，因此整个数组占用的内存大小依然是0。不用付出额外的内存代价，我们就通过for range方式实现times次快速迭代


数组不仅可以定义数值数组，还可以定义字符串数组、结构体数组、函数数组、接口数组、通道数组等

```
package main

import (
	"fmt"
	"image"
	"image/jpeg"
	"image/png"
	"io"
)

func main() {
	//字符串数组
	var s1 = [2]string{"hello", "world"}
	var s2 = [...]string{"hello", "go"}
	var s3 = [...]string{1: "hello", 0: "world"} //通过索引
	fmt.Println(s1, s2, s3)

	//结构体数组
	var line1 [2]image.Point
	var line2 = [...]image.Point{image.Point{X: 0, Y: 0}, image.Point{X: 1, Y: 1}}
	var line3 = [...]image.Point{{0, 0}, {1, 1}}
	fmt.Println(line1, line2, line3)

	//函数数组
	var decoder1 [2]func(io.Reader) (image.Image, error)
	var decoder2 = [...]func(io.Reader) (image.Image, error){
		png.Decode,
		jpeg.Decode,
	}
	fmt.Println(decoder1, decoder2)

	//接口数组
	var unknown1 [2]interface{}
	var unknown2 = [...]interface{}{123, "你好"}
	fmt.Println(unknown1, unknown2)

	//通道数组
	var chanList = [2]chan int{}
	fmt.Println(chanList)

	//空数组
	var d [0]int
	var e = [0]int{}
	var f = [...]int{}
	fmt.Println(d, e, f)
}
```

长度为0的数组（空数组）在内存中并不占用空间。空数组虽然很少直接使用，但是可以用于强调某种特有类型的操作时避免分配额外的内存空间，例如用于通道的同步操作:

```
ch1 := make(chan [0]int)
go func() {
	fmt.Println("ch1")
	ch1 <- [0]int{}
}()
<-ch1
```

在这里，我们并不关心通道中传输数据的真实类型，其中通道接收和发送操作只是用于消息的同步。对于这种场景，我们用空数组作为通道类型可以减少通道元素赋值时的开销。当然，一般更倾向于用无类型的匿名结构体代替空数组:

```
ch2 := make(chan struct{})
go func() {
	fmt.Println("ch2")
	ch2 <- struct{}{}  //struct{}部分是类型，{}部分表示对应的结构体值
}()
<-ch2
```

我们可以用fmt.Printf()函数提供的%T或%#v谓词语法来打印数组的类型和详细信息:
```
fmt.Printf("b: %T\n", b)
fmt.Printf("b: %#v\n", b)
```

在Go语言中，数组类型是切片和字符串等结构的基础


### 字符串

一个字符串是一个不可改变的字节序列，字符串通常是用来包含人类可读的文本数据

和数组不同的是，字符串的元素不可修改，是一个只读的字节数组。每个字符串的长度虽然也是固定的，但是字符串的长度并不是字符串类型的一部分

由于Go语言的源代码要求是UTF8编码，导致Go源代码中出现的字符串面值常量一般也是UTF8编码的。源代码中的文本字符串通常被解释为采用UTF8编码的Unicode码点（rune）序列

因为字节序列对应的是只读的字节序列，所以字符串可以包含任意的数据，包括字节值0

我们也可以用字符串表示GBK等非UTF8编码的数据，不过这时候将字符串看作是一个只读的二进制数组更准确，因为for range等语法并不能支持非UTF8编码的字符串的遍历

Go语言字符串的底层结构在reflect.StringHeader中定义:
```
type StringHeader struct {
	Data uintptr
	Len int
}
```

字符串结构由两个信息组成：第一个是字符串指向的底层字节数组；第二个是字符串的字节的长度。字符串其实是一个结构体，因此字符串的赋值操作也就是`reflect.StringHeader`结构体的复制过程，并不会涉及底层字节数组的复制

字符串虽然不是切片，但是支持切片操作，不同位置的切片底层访问的是同一块内存数据
```
s := "hello, world"
hello := s[:5]
world := s[7:]
s1 := "hello, world"[:5]
s2 := "hello, world"[7:]
```

字符串和数组类似，内置的len()函数返回字符串的长度

根据Go语言规范，Go语言的源文件都采用UTF8编码

下面的`hello,世界`字符串中包含了中文字符，可以通过打印转型为字节类型来查看字符底层对应的数据:
```
fmt.Printf("%#v\n", []byte("hello,世界"))
```

Go语言除了`for range`语法对UTF8字符串提供了特殊支持外，还对`字符串`和`[]rune`类型的相互转换提供了特殊的支持
```
[]rune("世界")
string([]rune{'世', '界'})
```

可以发现`[]rune`其实是`[]int32`类型，这里的`rune`只是`int32`类型的别名，并不是重新定义的类型。`rune`用于表示每个`Unicode`码点，目前只使用了21个位

字符串相关的强制类型转换主要涉及`[]byte`和`[]rune`两种类型。每个转换都可能隐含重新分配内存的代价，最坏的情况下它们运算的时间复杂度都是`O(n)`。不过字符串和`[]rune`的转换要更为特殊一些，因为一般这种强制类型转换要求两个类型的底层内存结构要尽量一致，显然它们底层对应的`[]byte`和`[]int32`类型是完全不同的内存结构，因此这种转换可能隐含重新分配内存的操作


### 切片
切片（slice）就是一种简化版的动态数组

因为动态数组的长度不固定，所以切片的长度自然也就不能是类型的组成部分了

数组虽然有适用的地方，但是数组的类型和操作都不够灵活，因此在Go代码中数组使用得并不多。而切片则使用得相当广泛，理解切片的原理和用法是Go程序员的必备技能

切片定义方式:
```
var (
	a []int    //nil切片,和nil相等,一般用来表示一个不存在的切片
	b = []int{}  //空切片,和nil不想等,一般用来表示一个空的集合
	c = []int{1, 2, 3}  //有3个元素的切片,len和cap都为3
	d = c[:2]  //有2个元素的切片,len为2,cap为3
	e = c[0:2:cap(c)]  //有2个元素的切片,len为2,cap为3
	f = c[:0]  //有0个元素的切片,len为0,cap为3
	g = make([]int, 3)  //有3个元素的切片,len为3,cap为3
	h = make([]int, 2, 3)  //有2个元素的切片,len为2,cap为3
	i = make([]int, 0, 3)  //有0个元素的切片,len为0,cap为3
)
```

和数组一样，内置的len()函数返回切片中有效元素的长度，内置的cap()函数返回切片容量大小，容量必须大于或等于切片的长度

切片可以和nil进行比较，只有当切片底层数据指针为空时切片本身才为nil,这时候切片的长度和容量信息将是无效的。如果有切片的底层数据指针为空，但是长度和容量不为0的情况，那么说明切片本身已经被损坏了

遍历切片的方式:
```
for i := range a {
	fmt.Println(i, a[i])
}

for i, v := range b {
	fmt.Println(i, v)
}

for i := 0; i < len(c); i++ {
	fmt.Println(i, c[i])
}
```

其实除了遍历之外，只要是切片的底层数据指针、长度和容量没有发生变化，对切片的遍历、元素的读取和修改就和数组一样。在对切片本身进行赋值或参数传递时，和数组指针的操作方式类似，但是只复制切片头信息`reflect.SliceHeader`，而不会复制底层的数据。对于类型，和数组的最大不同是，切片的类型和长度信息无关，只要是相同类型元素构成的切片均对应相同的切片类型

**添加切片元素**

内置的泛型函数`append()`可以在切片的尾部追加N个元素:
```
var a []int
a = append(a, 1)     //追加一个元素
a = append(a, 1, 2, 3)  //追加多个元素
a = append(a, []int{1, 2, 3}...)  //追加一个切片
```

不过要注意的是，在容量不足的情况下，append ()操作会导致重新分配内存，可能导致巨大的内存分配和复制数据的代价。即使容量足够，依然需要用append()函数的返回值来更新切片本身，因为新切片的长度已经发生了变化

除了在切片的尾部追加，还可以在切片的开头添加元素:
```
package main

import "fmt"

func main() {
	var a = []int{1, 2, 3}
	a = append([]int{0}, a...)          //在开头添加一个元素
	a = append([]int{-3, -2, -1}, a...) //在开头添加一个切片
	fmt.Println(a)
}
```

在开头一般都会导致内存的重新分配，而且会导致已有的元素全部复制一次。因此，从切片的开头添加元素的性能一般要比从尾部追加元素的性能差很多

由于append()函数返回新的切片，也就是它支持链式操作，因此我们可以将多个append ()操作组合起来，实现在切片中间插入元素:
```
package main

import "fmt"

func main() {
	var a = []int{1, 2, 3, 4}
	a = append(a[:2], append([]int{9}, a[2:]...)...)       //在第2个位置插入9
	a = append(a[:2], append([]int{1, 2, 3}, a[2:]...)...) //在第二个位置插入切片
	fmt.Println(a)
}
```

每个添加操作中的第二个`append()`调用都会创建一个临时切片，并将`a[i:]`的内容复制到新创建的切片中，然后将临时创建的切片再追加到`a[:i]`:
```
var a []int
a = append(a[:i], append([]int{x}, a[i:]...)...)
```

copy()和append()组合可以避免创建中间的临时切片，同样是完成添加元素的操作:
```
a = append(a, 0)    //切片扩容一个空间
copy(a[i+1:], a[i:])  //a[i:]向后移动一个位置
a[i] = x  //设置新添加的元素
```

第一句中的append()用于扩展切片的长度，为要插入的元素留出空间。第二句中的copy()操作将要插入位置开始之后的元素向后挪动一个位置。第三句真实地将新添加的元素赋值到对应的位置。操作语句虽然冗长了一点，但是相比前面的方法，可以减少中间创建的临时切片

用copy()和append()组合也可以实现在中间位置插入多个元素,也就是插入一个切片:
```
a = append(a, x...)      //为x切片扩展足够的空间
copy(a[i+len(x):], a[i:])  //a[i:]向后移动len(x)个位置
copy(a[i:], x)  //复制新添加的切片
```

**删除切片元素**

根据要删除元素的位置，有从开头位置删除、从中间位置删除和从尾部删除3种情况，其中删除切片尾部的元素最快:
```
a = []int{1, 2, 3}
a = a[:len(a)-1]  //删除尾部1个元素
a = a[:len(a)-N]  //删除尾部N个元素
```

删除开头的元素可以直接移动数据指针:
```
a = []int{1, 2, 3}
a = a[1:]   //删除开头1个元素
a = a[N:]   //删除开头N个元素
```

删除开头的元素也可以不移动数据指针，而将后面的数据向开头移动。可以用`append()`原地完成,`所谓原地完成是指在原有的切片数据对应的内存区间内完成，不会导致内存空间结构的变化`:
```
a = append(1, 2, 3)
a = append(a[:0], a[1:]...)  //删除开头1个元素
a = append(a[:0], a[N:]..)   //删除开头N个元素
```

也可以用copy()完成删除开头的元素:
```
a = []int{1, 2, 3}
a = a[:copy(a, a[1:])]  //删除开头一个元素
a = a[:copy(a, a[N:])]  //删除开头N个元素
```

对于删除中间的元素，需要对剩余的元素进行一次整体挪动，同样可以用`append()`或`copy()`原地完成:
```
a := []int{1, 2, 3, ...}
a = append(a[:i], a[i+1:]...)  //删除中间1个元素
a = append(a[:i], a[i+N:]...)  //删除中间N个元素
a = a[:i+copy(a[i:], a[i+1:])]  //删除中间1个元素
a = a[:i+copy(a[i:], a[i+N:])]  //删除中间N个元素
```

**切片内存技巧**

对于切片来说，len为0但是cap容量不为0的切片则是非常有用的特性。当然，如果len和cap都为0的话，则变成一个真正的空切片，虽然它并不是一个nil的切片。在判断一个切片是否为空时，一般通过len获取切片的长度来判断，一般很少将切片和nil做直接的比较

下面的`TrimSpace()`函数用于删除`[]byte`中的空格。函数实现利用了长度为0的切片的特性，实现高效而且简洁:
```
func TrimSpace(s []byte) []byte {
	b := s[:0]
	for _, x := range s {
		if x != ' ' {
			b = append(b, x)
		}
	}
	return b
}
```

其实类似的根据过滤条件原地删除切片元素的算法都可以采用类似的方式处理,因为是删除操作，所以不会出现内存不足的情形:
```
func Filter(s []byte, fn func(x byte) bool) []byte {
	b := s[:0]
	for _, x := range s {
		if !fn(x) {
			b = append(b, x)
		}
	}
	return b
}
```

切片高效操作的要点是要降低内存分配的次数，尽量保证append()操作不会超出cap的容量，降低触发内存分配的次数和每次分配内存的大小

**避免切片内存泄漏**

切片操作并不会复制底层的数据。底层的数组会被保存在内存中，直到它不再被引用。但是有时候可能会因为一个小的内存引用而导致底层整个数组处于被使用的状态，这会延迟垃圾回收器对底层数组的回收

例如，`FindPhoneNumber()`函数加载整个文件到内存，然后搜索第一个出现的电话号码，最后结果以切片方式返回:
```
func FindPhoneNumber(filename string) []byte {
	b, _ := ioutil.ReadFile(filename)
	return regexp.MustCompile("[0-9]+").Find(b)
}
```

这段代码返回的[]byte指向保存整个文件的数组。由于切片引用了整个原始数组，导致垃圾回收器不能及时释放底层数组的空间。一个小的需求可能导致需要长时间保存整个文件数据。这虽然不是传统意义上的内存泄漏，但是可能会降低系统的整体性能

要解决这个问题，可以将感兴趣的数据复制到一个新的切片中`数据的传值是Go语言编程的一个哲学，虽然传值有一定的代价，但是换取的好处是切断了对原始数据的依赖`:
```
func FindPhoneNumber(filename string) []byte {
	b, _ := ioutil.ReadFile(filename)
	b = regexp.MustCopile("[0-9]+").Find(b)
	return append([]byte{}, b...)
}
```

类似的问题在删除切片元素时可能会遇到。假设切片里存放的是指针对象，那么下面删除末尾的元素后，被删除的元素依然被切片底层数组引用，从而导致不能及时被垃圾回收器回收:
```
var a []*int{... }
a = a[:len(a)-1]  //被删除的最后一个元素依然被引用，可能导致垃圾回收器操作被阻碍
```

保险的方式是先将指向需要提前回收内存的指针设置为nil，保证垃圾回收器可以发现需要回收的对象，然后再进行切片的删除操作:
```
var a []*int{... }
a[len(a)-1] = nil  //垃圾回收器回收最后一个元素的内存
a = a[:len(a)-1]  //从切片删除最后一个元素
```

当然，如果切片存在的周期很短的话，可以不用刻意处理这个问题。因为如果切片本身已经可以被垃圾回收器回收的话，切片对应的每个元素自然也就可以被回收了

**切片类型强制转换**

待补充


## 函数-方法-接口
函数对应操作序列，是程序的基本组成元素。Go语言中的函数有具名和匿名之分：具名函数一般对应于包级的函数，是匿名函数的一种特例。当匿名函数引用了外部作用域中的变量时就成了`闭包函数`，**闭包函数是函数式编程语言的核心**。方法是绑定到一个具体类型的特殊函数，Go语言中的方法是依托于类型的，必须在编译时静态绑定。接口定义了方法的集合，这些方法依托于运行时的接口对象，因此接口对应的方法是在运行时动态绑定的。Go语言通过隐式接口机制实现了鸭子面向对象模型


### 函数
在Go语言中，**函数是第一类对象**，可以将函数保存到变量中。函数主要有具名和匿名之分，包级函数一般都是具名函数，具名函数是匿名函数的一种特例。当然，Go语言中每个类型还可以有自己的方法，方法其实也是函数的一种

```
//具名函数
func Add(a, b int) int {
	return a+b
}

//匿名函数
var Add = func(a, b int) int {
	return a+b
}
```

Go语言中的函数可以有多个参数和多个返回值，参数和返回值都是以**传值**的方式和被调用者交换数据。在语法上，函数还支持可变数量的参数，可变数量的参数必须是最后出现的参数，**可变数量的参数其实是一个切片类型的参数**

```
//多个参数和多返回值
func Swap(a, b int) (int, int) {
	return b, a
}

//可变数量的参数
func Sum(a int, more ...int) int {   //more对应 []int 切片类型
	for _, v := range more {
		a += v
	}
	return a
}
```

当可变参数是一个空接口类型时，调用者是否解包可变参数会导致不同的结果
```
func main() {
	var a = []interface{}{123, "abc"}
	Print(a...)  //123 abc
	Print(a)   // [123 abc]
}

func Print(a ...interface{}) {
	fmt.Println(a...)
}
```

第一个Print调用时传入的参数是`a...`，等价于直接调用`Print(123, "abc")`。第二个Print调用传入的是未解包的`a`，等价于直接调用`Print([]interface{}{123, "abc"} )`

不仅函数的参数可以有名字，也可以给函数的返回值命名:
```
func Find(m map[int]int, key int) (value int, ok bool) {
	value, ok = m[key]
	return
}
```

如果返回值命名了，可以通过名字来修改返回值，也可以通过defer语句在return语句之后修改返回值:
```
func Inc() (v int) {
	defer func() {
		v++
	}()
	return 42
}
```
其中`defer`语句延迟执行了一个匿名函数，因为这个匿名函数捕获了外部函数的局部变量`v`，这种函数我们一般称为`闭包`。`闭包对捕获的外部变量并不是以传值方式访问，而是以引用方式访问`

闭包的这种以引用方式访问外部变量的行为可能会导致一些隐含的问题:
```
func main() {
	for i := 0; i < 3; i++ {
		defer func() { fmt.Println(i) }()
	}
}

//输出结果为 3 3 3
```
因为是闭包，在`for`迭代语句中，每个`defer`语句延迟执行的函数引用的都是同一个`i`迭代变量，在循环结束后这个变量的值为3，因此最终输出的都是3

修复的思路是在每轮迭代中为每个defer语句的闭包函数生成独有的变量。可以用下面两种方式:
```
func main() {
	for i := 0; i < 3; i++ {
		i := i  //定义一个循环体内局部变量i
		defer func() { fmt.Println(i) }()
	}
}

func main() {
	for i := 0; i < 3; i++ {
		defer func(i int) { fmt.Println(i) }(i)
		//通过函数传入i
		//defer 语句会马上对调用参数求值
	}
}
```

第一种方法是在循环体内部再定义一个局部变量，这样每次迭代`defer`语句的闭包函数捕获的都是不同的变量，这些变量的值对应迭代时的值。第二种方式是将迭代变量通过闭包函数的参数传入，`defer`语句会马上对调用参数求值。两种方式都是可以工作的。不过一般来说,在`for`循环内部执行`defer`语句并不是一个好的习惯


Go语言中，如果以切片为参数调用函数，有时候会给人一种参数采用了传引用的方式的假象,因为在被调用函数内部可以修改传入的切片的元素。其实，任何可以通过函数参数修改调用参数的情形，都是因为函数参数中显式或隐式传入了指针参数

函数参数传值的规范更准确说是只针对**数据结构中固定的部分传值**，例如字符串或切片对应结构体中的指针和字符串长度结构体传值，但是并不包含指针间接指向的内容

将切片类型的参数替换为类似`reflect.SliceHeader`结构体就能很好理解切片传值的含义了:
```
func twice(x []int) {
	for i := range x {
		x[i] *= 2
	}
}

type IntSliceHeader struct {
	Data []int
	Len int
	Cap int
}

func twice(x IntSliceHeader) {
	for i := 0; i < x.Len; i++ {
		x.Data[i] *= 2
	}
}
```
因为切片中的底层数组部分通过隐式指针传递,指针本身依然是传值的，但是指针指向的却是同一份的数据,所以被调用函数可以通过指针修改调用参数切片中的数据. 

除数据之外，切片结构还包含了切片长度和切片容量信息，这两个信息也是传值的。如果被调用函数中修改了`Len`或`Cap`信息，就无法反映到调用参数的切片中，这时候我们一般会通过返回修改后的切片来更新之前的切片。这也是内置的`append ()`必须要返回一个切片的原因

Go语言中，函数还可以直接或间接地调用自己，也就是支持递归调用。Go语言函数的递归调用深度在逻辑上没有限制，函数调用的栈是不会出现溢出错误的，因为Go语言运行时会根据需要动态地调整函数栈的大小

因为Go语言函数的栈会自动调整大小，所以普通Go程序员已经很少需要关心栈的运行机制了。在Go语言规范中甚至故意没有讲到栈和堆的概念。我们无法知道函数参数或局部变量到底是保存在栈中还是堆中

```
func f(x int) *int {
	return &x
}

func g() int {
	x = new(int)
	return *int
}
```
第一个函数直接返回了函数参数变量的地址——这似乎是不可以的，因为如果参数变量在栈上，函数返回之后栈变量就失效了，返回的地址自然也应该失效了。但是Go语言的编译器和运行时比我们聪明得多，它会保证指针指向的变量在合适的地方

第二个函数，内部虽然调用`new()`函数创建了`*int`类型的指针对象，但是依然不知道它具体保存在哪里。对于有C/C++编程经验的程序员需要强调的是：不用关心Go语言中函数栈和堆的问题，编译器和运行时会帮我们搞定；同样不要假设变量在内存中的位置是固定不变的，指针随时可能会变化，特别是在你不期望它变化的时候


### 方法
方法一般是面向对象编程`（Object-Oriented Programming，OOP）`的一个特性，在C++语言中方法对应一个类对象的成员函数，是关联到具体对象上的虚表中的

但是Go语言的方法却是关联到类型的，这样可以在编译阶段完成方法的静态绑定

一个面向对象的程序会用方法来表达其属性对应的操作，这样使用这个对象的用户就不需要直接去操作对象，而是借助方法来做这些事情

面向对象编程进入主流开发领域一般认为是从C++开始的，C++就是在兼容C语言的基础之上支持了类等面向对象的特性。然后Java编程则号称是纯粹的面向对象语言，因为Java中函数是不能独立存在的，每个函数都必然是属于某个类的

面向对象编程更多的只是一种思想，很多号称支持面向对象编程的语言只是将经常用到的特性内置到语言中了而已。Go语言的祖先C语言虽然不是一个支持面向对象的语言，但是C语言的标准库中的File相关的函数也用到了面向对象编程的思想

下面我们实现一组C语言风格的File函数:
```
//文件对象
type File struct {
	fd int
}

//打开文件
func OpenFile(name string) (f *File, err error) {
	//...
}

//关闭文件
func CloseFile(f *File) error {
	//...
}

//读取文件
func ReadFile(f *File, offset int64, data []byte) int {
	//...
}
```
其中`OpenFile()`类似于构造函数，用于打开文件对象，`CloseFile()`类似于析构函数，用于关闭文件对象，`ReadFile()`则类似于普通的成员函数，这3个函数都是普通函数

`CloseFile()`和`ReadFile()`作为普通函数，需要占用包级空间中的名字资源。不过`CloseFile()`和`ReadFile()`函数只是针对`File`类型对象的操作，这时候我们更希望这类函数和操作对象的类型紧密绑定在一起

Go语言中的做法是将函数`CloseFile()`和`ReadFile()`的第一个参数移动到函数名的开头:
```
//关闭文件
func (f *File) CloseFile() error {
	//...
}

//读取文件数据
func (f *File) ReadFile(offset int64, data []byte) int {
	//...
}
```
将第一个函数参数移动到函数前面，从代码角度看虽然只是一个小的改动，但是从编程哲学角度看，Go语言已经是进入面向对象语言的行列了

我们可以给任何自定义类型添加一个或多个方法。每种类型对应的方法必须和类型的定义在同一个包中，因此是无法给`int`这类`内置类型`添加方法的,因为方法的定义和类型的定义不在一个包中

对于给定的类型，每个方法的名字必须是唯一的，同时方法和函数一样也不支持`重载`

方法由函数演变而来，只是将函数的第一个对象参数移动到了函数名前面了而已。因此我们依然可以按照原始的过程式思维来使用方法。通过称为方法表达式的特性可以将方法还原为普通类型的函数:
```
//不依赖具体的文件对象
//func CloseFile(f *File) error
var CloseFile = (*File).Close

//func ReadFile(f *File, offset int64, data []byte) int
var ReadFile = (*File).Read

//文件处理
f, _ := OpenFile("go.txt")
ReadFile(f, 0, data)
CloseFile(f)
```

在有些场景更关心一组相似的操作。例如，`Read()`读取一些数组，然后调用`Close()`关闭。此时的环境中，用户并不关心操作对象的类型，只要能满足通用的`Read()`和`Close()`行为就可以了。不过在方法表达式中，因为得到的`ReadFile()`和`CloseFile()`函数参数中含有`File`这个特有的类型参数，这使得`File`相关的方法无法与其他不是`File`类型但是有着相同`Read()`和`Close()`方法的对象无缝适配。这种小困难难不倒Go语言程序员，我们可以通过结合`闭包特性`来消除方法表达式中第一个参数类型的差异:
```
//先打开文件对象
f, _ := OpenFile("go.txt")
//绑定到 f 对象向
//func Close() error
var Close = func Close() error {
	return (*File).Close(f)
}

//绑定到了f对象上
//func Read(offset int64, data []byte) int
var Read = func Read(offset int64, data []byte) int {
	return (*File).Read(f, offset, data)
}

//文件处理
Read(0, data)
Close()
```

这刚好是方法值也要解决的问题。我们用方法值特性可以简化实现:
```
f, _ := OpenFile("go.txt")
//方法值，绑定到 f 对象
//func Close() error
var Close = f.Close

//func Read(offset int64, data []byte) int
var Read = f.Read

//文件处理
Read(0, data)
Close()
```

Go语言不支持传统面向对象中的继承特性，而是以自己特有的组合方式支持了方法的继承。Go语言中，通过在结构体内置匿名的成员来实现继承:
```
import "image/color"
type Point struct{X, Y float64}
type ColorPoint struct{
	Point
	Color color.RGBA
}
```
虽然我们可以将ColoredPoint定义为一个有3个字段的扁平结构的结构体，但是这里将Point嵌入ColoredPoint来提供X和Y这两个字段:
```
var cp ColorPoint
cp.X = 1
fmt.Println(cp.Point.X)
cp.Point.Y = 2
fmt.Println(cp.Y)
```
通过嵌入匿名的成员，不仅可以继承匿名成员的内部成员，而且可以继承匿名成员类型所对应的方法。我们一般会将Point看作基类，把ColoredPoint看作Point的继承类或子类。不过这种方式继承的方法并不能实现C++中虚函数的多态特性。所有继承来的方法的接收者参数依然是那个匿名成员本身，而不是当前的变量:
```
type Cache struct {
	m map[string]string
	sync.Mutex
}

func (p *Cache) Lookup(key string) string {
	p.Lock()
	defer p.Unlock()
	return p.m[key]
}
```
Cache结构体类型通过嵌入一个匿名的sync.Mutex来继承它的方法Lock()和Unlock()。但是在调用p.Lock()和p.Unlock()时，p并不是方法Lock()和Unlock()的真正接收者，而是会将它们展开为p.Mutex.Lock()和p.Mutex.Unlock()调用。这种展开是编译期完成的，并没有运行时代价

在传统的面向对象语言（例如C++或Java）的继承中，子类的方法是在运行时动态绑定到对象的，因此基类实现的某些方法看到的`this`可能不是基类类型对应的对象，这个特性会导致基类方法运行的不确定性。而在Go语言通过嵌入匿名的成员来`继承`的基类方法，`this`就是实现该方法的类型的对象，Go语言中方法是编译时静态绑定的。如果需要虚函数的多态特性，我们需要借助Go语言接口来实现

### 接口
一般静态编程语言都有着严格的类型系统，这使得编译器可以深入检查程序员有没有作出什么出格的举动。但是，过于严格的类型系统却会使得编程太过烦琐，让程序员把时间都浪费在了和编译器的斗争中

Go语言试图让程序员能在安全和灵活的编程之间取得一个平衡。它在提供严格的类型检查的同时，通过接口类型实现了对`鸭子类型`的支持，使得安全动态的编程变得相对容易

Go的接口类型是对其他类型行为的抽象和概括，因为接口类型不会和特定的实现细节绑定在一起，通过这种抽象的方式我们可以让对象更加灵活和更具有适应能力。很多面向对象的语言都有相似的接口概念，但Go语言中接口类型的独特之处在于它是满足隐式实现的鸭子类型

所谓鸭子类型说的是：**只要走起路来像鸭子、叫起来也像鸭子，那么就可以把它当作鸭子**

Go语言中的面向对象就是如此，如果一个对象只要看起来像是某种接口类型的实现，那么它就可以作为该接口类型使用

这种设计可以让你创建一个新的接口类型满足已经存在的具体类型却不用去破坏这些类型原有的定义

当使用的类型来自不受我们控制的包时这种设计尤其灵活有用。Go语言的接口类型是延迟绑定，可以实现类似虚函数的多态功能

接口在Go语言中无处不在，在“Hello, World”的例子中，fmt.Printf()函数的设计就是完全基于接口的，它的真正功能由fmt.Fprintf()函数完成。用于表示错误的error类型更是内置的接口类型。在C语言中，printf只能将几种有限的基础数据类型打印到文件对象中。但是Go语言由于灵活的接口特性，fmt.Fprintf可以向任何自定义的输出流对象打印，可以打印到文件或标准输出，也可以打印到网络，甚至可以打印到一个压缩文件；同时，打印的数据也不仅局限于语言内置的基础类型，任意隐式满足fmt.Stringer接口的对象都可以打印，不满足fmt.Stringer接口的依然可以通过反射的技术打印。fmt.Fprintf()函数的签名如下:
```
func Fprintf(w io.Writer, format string, args ...interface{}) (int, error)
```

其中io.Writer是用于输出的接口，error是内置的错误接口，它们的定义如下:
```
type io.Writer interface {
	Write(p []byte) (n int, err error)
}

type error interface {
	Error() string
}
```

我们可以通过定制自己的输出对象，将每个字符转换为大写字符后输出:
```
type UpperWriter struct {
	io.Writer
}

func (p *UpperWriter) Write(data []byte) (n int, err error) {
	return p.Writer.Write(bytes.ToUpper(data))
}

func main() {
	fmt.Fprintln(&UpperWriter{os.Stdout}, "hello, go")
}
```

当然，我们也可以定义自己的打印格式来实现将每个字符转换为大写字符后输出的效果。对于每个要打印的对象，如果满足了`fmt.Stringer`接口，则默认使用对象的`String()`方法返回的结果打印:
```
type UpperString string
func (s UpperString) String() string {
	return strings.ToUpper(string(s))
}

type fmt.Stringer interface {
	String() string
}

func main() {
	fmt.Fprintln(os.Stdout, UpperString("hello, go"))
}
```

Go语言中，对于基础类型（非接口类型）不支持隐式的转换，我们无法将一个int类型的值直接赋值给int64类型的变量，也无法将int类型的值赋值给底层是int类型的新定义命名类型的变量

Go语言对基础类型的类型一致性要求可谓是非常的严格，但是Go语言对于接口类型的转换则非常灵活。对象和接口之间的转换、接口和接口之间的转换都可能是隐式的转换
```
var (
	a io.ReadCloser = (*os.File)(f)  //隐式转换
	b io.Reader = a  //隐式转换
	c io.Closer = a  //隐式转换
	d io.Reader = c.(io.Reader)  //显式转换
)
```

有时候对象和接口之间太灵活了，需要人为地限制这种无意之间的适配。常见的做法是定义一个特殊方法来区分接口。例如runtime包中的Error接口就定义了一个特有的`RuntimeError()`方法，用于避免其他类型无意中适配了该接口
```
type runtime.Error interface {
	error
	RuntimeError()
}
```

在Protobuf中，Message接口也采用了类似的方法，也定义了一个特有的`ProtoMessage`，用于避免其他类型无意中适配了该接口
```
type proto.Message interface {
	Reset()
	String() string
	ProtoMessage()
}
```

不过这种做法只是`君子协定`，如果有人故意伪造一个`proto.Message`接口也是很容易的。再严格一点的做法是给接口定义一个私有方法。只有满足了这个私有方法的对象才可能满足这个接口，而私有方法的名字是包含包的绝对路径名的，因此只有在包内部实现这个私有方法才能满足这个接口。测试包中的`testing.TB`接口就是采用类似的技术
```
type testing.TB interface {
	Error(args ...interface{})
	Errorf(format string, args ...interface{})
	private()
}
```

不过这种通过私有方法禁止外部对象实现接口的做法也是有代价的,首先是这个接口只能在包内部使用，外部包在正常情况下是无法直接创建满足该接口对象的；其次，这种防护措施也不是绝对的，恶意的用户依然可以绕过这种保护机制

通过在结构体中嵌入匿名类型成员，可以继承匿名类型的方法。其实这个被嵌入的匿名成员不一定是普通类型，也可以是接口类型。我们可以通过嵌入匿名的testing.TB接口来伪造私有方法，因为接口方法是延迟绑定，所以编译时私有方法是否真的存在并不重要
```
package main
import (
	"fmt"
	"testing"
)

type TB struct {
	testing.TB
}

func (p *TB) Fatal(args ...interface{}) {
	fmt.Println("TB.Fatal disabled")
}

func main() {
	var tb testing.TB = new(TB)
	tb.Fatal("hello, go")
}
```
我们在自己的TB结构体类型中重新实现了`Fatal()`方法，然后通过将对象隐式转换为`testing.TB`接口类型（因为内嵌了匿名的testing.TB对象，所以是满足testing.TB接口的），再通过`testing.TB`接口来调用自己的`Fatal()`方法

这种通过嵌入匿名接口或嵌入匿名指针对象来实现继承的做法其实是一种纯虚继承，继承的只是接口指定的规范，真正的实现在运行的时候才被注入

模拟实现一个gRPC的插件:
```
type grpcPlugin struct {
	*generator.Generator
}

func (p *grpcPlugin) Name() string {return "grpc"}

func (p *grpcPlugin) Init(g *generator.Generator) {
	p.Generator = g
}

func (p *grpcPlugin) GenerateImports(file *generator.FileDescriptor) {
	if len(file.Service) == 0 {
		return
	}
	p.P(`import "google.golang.org/grpc" `)
}
```

构造的grpcPlugin类型对象必须满足generate.Plugin接口:
```
type Plugin interface {
	Name() string
	Init(g *Generator)
	Generate(file *FileDescriptor)
	GenerateImports(file *FileDescriptor)
}
```
`generate.Plugin`接口对应的`grpcPlugin`类型的`GenerateImports()`方法中使用的`p.P(...)`函数，却是通过`Init()`函数注入的`generator.Generator`对象实现。这里的`generator.Generator`对应一个具体类型，但是如果`generator.Generator`是接口类型，我们甚至可以传入直接的实现

Go语言通过几种简单特性的组合，就轻易实现了鸭子面向对象和虚拟继承等高级特性，真的是不可思议