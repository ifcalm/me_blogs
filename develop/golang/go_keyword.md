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
