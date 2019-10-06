# golang 的 25 个关键字

## 1. var
`var` 是用于定义变量的

常见的有如下几种形式：
```
var age int   //定义变量
var age int = 24    //定义变量的同时进行初始化

//同时定义多个变量
var age1, ag2 int = 23, 24

//同时定义不同类型的多个变量
var (
    age int = 24
    name string = "Lss"
)

//定义变量时省略类型
var age1, age2 = 23, 24
```

变量还可以进行简短声明，使用`:=` 方式
```
a := 24
s := "Lss"
arr := [5]int{}
```

#### var 与 := 的区别
简短声明 `:=` 只能出现在函数内部，而`var` 可以出现在函数内外，一般用 var 方式来定义全局变量。

## 2. const
const 用于声明一个常量，一旦创建，不可赋值修改。

```
const Pi float64 = 3.1415

const zero = 0.0

const (
    size int64 = 1024
    eof = -1
)

const u, v float64 = 0, 3

const a, b, c = 3, 4, "foo"
```

go 的常量定义可以限定常量类型，也可以不指定。

如果没有指定类型，那么它是无类型常量。

只要这个常量在相应类型的值域范围内，就可以作为该类型的常量，比如常量-12，它可以赋值给int、uint、int32、int64、float32、float64、complex64、complex128等类型的变量

常量定义的右值也可以是一个在编译期运算的常量表达式：`const mask = 1 << 3`

由于常量的赋值是一个编译期行为，所以右值不能出现任何需要运行期才能得出结果的表达式，比如试图以如下方式定义常量就会导致编译错误：

`const Home = os.GetEnv("HOME")`

原因很简单，os.GetEnv()只有在运行期才能知道返回结果，在编译期并不能确定，所以无法作为常量定义的右值。


## 3. package
go 用 package 来定义一个包，包是函数和数据的集合。文件名不需要和包名一致。包名的约定是使用小写字符，包可以由多个文件组成。

当函数或者变量首字母大写时，会被导出，可在外部直接调用。

golang package是基本的管理单元,同一个package下面，可以有非常多的不同文件，只要 每个文件的头部都有 如 `package xxx` 的相同name，

就可以 在主方法中使用 `xxx.Method()`调用不同文件中的方法了。

文件夹名字可以和这个package 名称不一致

```
package main
```

## 4. import
import 用于引入一个包

```
import "fmt"
import "time"

import (
    "fmt"
    "time"
)
```

`import f "fmt"` 导入fmt，并给他启别名ｆ

`import . "fmt"` 将fmt启用别名"."，这样就可以直接使用其内容，而不用再添加fmt，如fmt.Println可以直接写成Println

`import  _ "fmt"` 表示不使用该包，而是只是使用该包的init函数，并不显示的使用该包的其他内容。注意：这种形式的import，当import时就执行了fmt包中的init函数，而不能够使用该包的其他函数。

## 5. map
map 是 go 内置 的关联数据类型，也可以称为 哈希 或者 字典。
```
m := make(map[string]int) //创建一个空 map

//先声明，在使用 make函数创建
var m1 map[string]string
m1 = make(map[string]string)

//赋值
m1["a"] = "aa"
m1["b"] = "bb"

//初始化 + 赋值
m2 := map[string]string{
    "a": "aa",
    "b": "bb",
}

//查找键值是否存在
if v, ok := m1["a"]; ok {
    fmt.Println(v)
} else {
    fmt.Println("key Not Found")
}

//遍历map
for k, v := range m1 {
    fmt.Println(k, v)
}

//删除键值
delete(m1, "a")
```

## 6. func
func 用于定义函数

```
func test(a, b int) int {
    return a + b
}
```

## 7. return
return 用在函数内部，退出函数执行过程
* 退出函数执行过程，不指定返回值
  通常有两种情况不需要指定返回值退出函数执行过程。第一是：函数没有返回值；第二是：函数返回值有变量名，不需要显示的指定返回值。下边通过一个示例来说明这两种情况：
  ```
  func say(flag bool) {
      if !flag {
          fmt.Println("false")
          return
      }
      fmt.Println("没有返回值")
  }

  func getStatus() (num int) {
      num = 100       //num 是在返回值中定义的变量
      return
  }
  ```
  上边的示例代码中，say是一个没有返回值的函数，这个函数中的return就是一个不带返回值的退出。getStatus是带一个返回值的函数，由于在返回值中定义了变量，所以，在函数退出时，可以不用显示的在return后边指定函数返回值，函数调用结束后，自动将之前定义的返回值变量作为这个函数的返回结果。

* 退出函数执行过程，并指定返回值
  当函数有返回值时，如果返回值没有定义变量，那么一定要使用 return 加上返回值退出函数。
  ```
  func getMsg() string {
      return "hello"
  }
  ```

* 在返回值中定义变量，而 return 其他结果
  在函数定义时，虽然在返回值中定义了变量，但是在return出函数时，指定另外的变量作为返回结果，这样也是可行的，但是意义却不大。示例代码如下：
  ```
  func getMsg() (msg string) {
      msg = "world"
      return "hello"
  }
  ```
  在函数返回值中定义变量，只是说明这个函数在return时，没有显示指定返回对象的情况下，默认使用返回值中的变量作为函数的返回结果，如果return时显示的指定了返回对象，那么以显示指定的返回对象作为函数执行的返回结果

* 在函数递归调用中使用 return
  递归调用，就是函数在函数体内调用自身。函数在没有结束之前，调用自身，表示嵌套层级增加一层。各个调用层中，函数执行完成，表示这一层执行完成。在函数递归调用中使用return，只会退出当前这一层的函数执行，并不会结束其他层的函数执行过程。使用return可以结束函数执行过程，阻止函数体内后边的代码执行。将对自身的调用写在return后边，可以停止函数的递归调用，如下边示例所示，当index等于5时，结束了函数的执行过程，后边代码中对自身的调用便不会执行。

  ```
  func getMsg(index int) {
      if index == 5 {
          fmt.Println("递归调用嵌套层级达到5层，结束递归")
          return
      }

      fmt.Println("递归调用层级是：", index)
      getMsg(index + 1)
  }
  ```

## 8. defer
defer 用于资源的释放，会在函数返回之前进行调用
```
func test() {
    f, err := os.Open(filename)
    if err != nil {
        panic(err)
    }
    defer f.Close()
}
```
如果有多个defer表达式，调用顺序类似于栈，越后面的defer表达式越先被调用，即后入先出的规则。
```
func test() {
    defer fmt.Println(1)
    defer fmt.Println(2)
}

输出结果为：
2
1
```

在golang当中，defer代码块会在函数调用链表中增加一个函数调用。这个函数调用不是普通的函数调用，而是会在函数正常返回，也就是return之后添加一个函数调用。因此，defer通常用来释放函数内部变量

### defer 使用规则
 * 当 defer 被声明时，其参数就会被实时解析
  
   ```
   func a() {
       i := 0
       defer fmt.Println(i)
       i++
       return
   }

   defer 函数会在 return 之后被调用，那么这段函数执行完之后，是不用应该输出1呢？
   结果输出的是 0，
   这是因为虽然我们在defer后面定义的是一个带变量的函数: fmt.Println(i). 但这个变量(i)在defer被声明的时候，就已经确定其确定的值了。
   ```
   我们继续定义一个 defer ：
   ```
   func a() {
       i := 0

       defer fmt.Println(i) //输出0，因为此时 i 就是0

       i++

       defer fmt.Println(i) //输出1，因为此时 i 为 1

       return
   }
   ```
   通过运行结果，可以看到defer输出的值，就是定义时的值。而不是defer真正执行时的变量值

 * defer 执行顺序是先进后出
   当同时定义了多个defer代码块时，golang按照先定义后执行的顺序依次调用defer
   ```
   func b() {
       for i := 0; i < 4; i++ {
           defer fmt.Println(i)
       }
   }

   按照先进后出的原则，依次输出 3 2 1 0
   ```
 * defer 可以读取有名返回值
   ```
   func c() (i int) {
       defer func() { i++ }()
       return 1
   }

   当执行return 1 之后，i的值就是1. 此时此刻，defer代码块开始执行，对i进行自增操作。 因此输出2.
   ```
   我们说过defer是在return调用之后才执行的。 这里需要明确的是defer代码块的作用域仍然在函数之内，结合上面的函数也就是说，defer的作用域仍然在c函数之内。因此defer仍然可以读取c函数内的变量


## 9. type
type 关键词 主要是用于定义类型的

type 有如下几种用法：
* 定义结构体

  结构体是用户自定义的一种抽象的数据结构
  ```
  type Lss struct {
      name string
      age int
      heght float64
  }
  ```

* 定义接口
  
  ```
  type Lss interface {
      Read()
      Write()
  }
  ```

* 类型定义
  
  使用类型定义定义出来的类型与原类型不相同，所以不能使用新类型变量赋值给原类型变量
  ```
  type lss string
  根据string类型，定义一种新的类型，新类型名称是lss

  类型定义可以在原类型的基础上创造出新的类型，有些场合下可以使代码更加简洁
  ```

* 类型别名
  
  使用类型别名定义出来的类型与原类型一样，即可以与原类型变量互相赋值，又拥有了原类型的所有方法集
  ```
  type lss = string
  ```
  类型别名与类型定义不同之处在于，使用类型别名需要在别名和原类型之间加上赋值符号(=); 使用类型别名定义的类型与原类型等价，而使用类型定义出来的类型是一种新的类型

  给类型别名新增方法，会添加到原类型方法集中

* 类型查询
  
  类型查询，就是根据变量，查询这个变量的类型。为什么会有这样的需求呢？goalng中有一个特殊的类型interface{}，这个类型可以被任何类型的变量赋值，如果想要知道到底是哪个类型的变量赋值给了interface{}类型变量，就需要使用类型查询来解决这个需求
  ```
  func main() {
      //定义一个 interface{}类型变量
      var a interface{} = "lss"

      //在switch中使用 变量名.(type) 查询是由哪个类型数据赋值
      switch v := a.(type) {
          case string:
            fmt.Println("字符串")
          case int:
            fmt.Println("整型")
          default:
            fmt.Println("其他类型")
      }
  }

  如果使用.(type)查询类型的变量不是interface{}类型，则在编译时会报错误

  所以，使用type进行类型查询时，只能在switch中使用，且使用类型查询的变量类型必须是interface{}
  ```

## 10. struct
go 的结构体 是各个字段 类型 的集合，是值类型

struct 的基本用法
```
type Person struct {
    Name string
    Age int
}

func main() {
    p := Persion{
        Name: "lss",
        Age: 20,
    }
    p.Age = 24
    fmt.Println(p)
}
```

struct 支持嵌入式结构
```
type Person struct {
    Name string
}
type User struct {
    Person
    Age int
}

func main() {
    u := User {
        Person: Person {
            Name: "lss",
        },
        Age: 24,
    }
    fmt.Println(u.Age)
    fmt.Println(u.Person.Name)
}
```
通过变量名，使用逗号(.)，可以访问结构体类型中的字段，或为字段赋值，也可以对字段进行取址(&)操作。

如果想在一个包中访问另一个包中结构体的字段，则必须是大写字母开头的变量，即可导出的变量

除字段名称和数据类型外，还可以使用反引号为结构体字段声明元信息，这种元信息称为Tag，用于编译阶段关联到字段当中
```
type Member struct {
    Id     int    `json:"id"`
    Name   string `json:"name"`
    Email  string `json:"email"`
    Gender int    `json:"gender"`
    Age    int    `json:"age"`
}
```

结构体与数组一样，是复合类型，无论是作为实参传递给函数时，还是赋值给其他变量，都是值传递，即复制一个副本。


## 11. interface
interface 是一种具有一组方法的类型，这些方法定义了 interface 的行为。

如果一个类型实现了一个 interface 中的所有方法，我们就说实现了该 interface。

所以所有类型都实现了 empty interface，因为任何一种类型至少实现了 0 个方法。go 没有显式的关键字用来实现 interface，只需要实现 interface 包含的方法即可。

接口的定义与基本操作：
```
type Dog interface {
    Category()
}

type Lss struct {
    Name string
}

func (l Lss) Category() {
    fmt.Println("帅帅")
}

func test(a Dog) {
    fmt.Println("ok")
}

func main() {
    l : Lss{"很帅"}
    l.Category()
    test(l)
}
```

当我们需要使用复杂结构关系的时候，我们就会需要用到嵌入接口

```
type Dog interface {
    Animal
}

type Animal interface {
    Category()
}
```

#### 类型断言
一个 interface 被多种类型实现时，有时候我们需要区分 interface 的变量究竟存储哪种类型的值

`value, ok := em.(T)`，em 是 interface 类型的变量，T代表要断言的类型，value 是 interface 变量存储的值，ok 是 bool 类型表示是否为该断言的类型 T

#### 空接口
空接口 interface{} 没有任何方法签名，也就意味着任何类型都实现了空接口。其作用类似于面向对象语言中的根对象 Object 
```
func Print(v interface{}) {
  fmt.Println(v)
}

func main() {
  Print(1)
  Print("Hello, World")
}
```

## 12. for
for 是 Go 中唯一的循环结构

以下是最基本的几种使用方式：

```
i := 1
for i <= 3 {
    fmt.Println(i)
    i = i + 1
}

类似于 while
```

```
for i := 0; i <= 10; i++ {
    fmt.Println(i)
}

经典的 for 循环
```

```
for {
    fmt.Println("loop")
    break
}

for 循环将一直执行，直到在循环体内使用了 break 或者 return 来跳出循环
```

## 13-14. if , else
if 支持初始化语句，可定义代码块局部变量

```
if num := 9; num < 0 {
    fmt.Println(1)
} else if num < 10 {
    fmt.Println(2)
} else {
    fmt.Println(3)
}
```

## 15-16-17. switch , case , default
Go的switch非常灵活，表达式不必是常量或整数，执行的过程从上至下，直到找到匹配项；而如果switch没有表达式，它会匹配true。 Go里面switch默认相当于每个case最后带有break，匹配成功后不会自动向下执行其他case，而是跳出整个switch

```
switch i {
    case 0:
        fmt.Println("0")
    case 1:
        fmt.Println("1")
    case 2,3, 4:
        fmt.Println("2,3,4")
    default:
        fmt.Println("Default")
}
```

switch后面的表达式不是必需的

```
switch {
    case 0 <= num && num <= 3:
        fmt.Println("0-3")
    case 4 <= num && num <= 6:
        fmt.Println("4-6")
}

可以不设定switch之后的条件表达式，在此种情况下，整个switch结构与多个if...else...的逻辑作用等同

只有在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case
```

## 18. fallthrough
在switch中，使用 `fallthrough` 可以强制执行后面的case代码

```
switch i { 
    case 0: 
        fmt.Printf("0")
        fallthrough
    case 1: 
        fmt.Printf("1")
        fallthrough
    case 2: 
        fallthrough 
    case 3: 
        fmt.Printf("3")
        fallthrough
    case 4, 5, 6: 
        fmt.Printf("4, 5, 6") 
    default: 
        fmt.Printf("Default") 
} 
```

## 19-20. break , continue
break 是跳出当前循环，并开始执行循环之后的语句

continue 是跳过该次循环，继续下次循环

```
func main() {
    var a int = 10

    for a < 20 {
        fmt.Println(a)
        a++
        if a > 15 {
            break    //使用 break 语句跳出循环
        }
    }
}
```

break label语句：我们在 for 多层嵌套时，有时候需要直接跳出所有嵌套循环，这时候可以用到 go 的 label 了
```
func main() {
    fmt.Println("1")

Exit:
    for i := 0; i < 9; i++ {
        for j := 0; j < 9; j++ {
            if i + j > 15 {
                fmt.Println("exit")
                break Exit
            }
        }
    }
}

label要写在for循环的开始而不是结束的地方
```

continue 不是跳出循环，而是跳过当前循环执行下一次循环语句

```
func main() {
    var a int = 10

    for a < 20 {
        if a == 15 {
            a = a + 1
            continue
        }
        fmt.Printlf("a value is : %d\n", a)
        a++
    }
}
```

配合标签，break 和 continue 可在多级嵌套循环中跳出
```
func main() {
L1:
	for x := 0; x < 3; x++ {
	L2:
		for y := 0; y < 5; y++ {
			if y > 2 {
				continue L2
			}
			if x > 1 {
				break L1
			}

			print(x, ":", y, " ")
		}
		println()
	}
}
```

break 可用于 for、switch、select，而 continue 仅能用于 for 循环

## 21. goto
goto 语句将控制转移到被标记的语句，goto 是调整执行位置

Go 语言的 goto 语句可以无条件地转移到过程中指定的行。

goto语句通常与条件语句配合使用。可用来实现条件转移， 构成循环，跳出循环体等功能。

但是，在结构化程序设计中一般不主张使用goto语句， 以免造成程序流程的混乱，使理解和调试程序都产生困难

```
func main() {
    var a int = 10

    LOOP: for a < 20 {
        if a == 15 {
            a = a + 1
            goto LOOP
        }

        fmt.Println(a)
        a++
    }
}
```

Golang支持在函数内 goto 跳转

## 22. range
Golang range类似迭代器操作，返回 (索引, 值) 或 (键, 值)。

range 会复制对象

for 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：
```
for key, value := range oldMap {
    newMap[key] = value
}
```

可以忽略不想要的返回值，或使用 "_" 这个特殊变量

```
func main() {
    s := "abc"

    for i := range s {
        println(s[i])
    }

    for _, c := range s {
        println(c)
    }

    for range s {

    }

    m := map[string]int{"a": 1, "b": 2}
    for k, v := range m {
        println(k, v)
    }
}
```

## 23. go
