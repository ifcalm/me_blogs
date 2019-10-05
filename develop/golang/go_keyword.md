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
