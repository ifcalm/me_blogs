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