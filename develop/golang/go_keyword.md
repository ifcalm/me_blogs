# golang 的 25 个关键字

### var
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

### const