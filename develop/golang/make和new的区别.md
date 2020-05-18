## make - new

### new
`new` 只能传递一个参数，该参数为一个任意类型，可以是Go内建的类型，也可以是自定义的类型

`new` 函数做了如下事情:
- 分配内存
- 设置零值
- 返回指针（重要）

```
package main

import (
	"fmt"
)

type People struct {
	name string
	age  int
}

func main() {
	num := new(int)
	fmt.Println(*num)

	p := new(People)
	p.name = "lss"
	fmt.Println(p)
}
```
输出结果如下：
```
0
&{lss 0}
```

### make
内建函数 `make` 用来 为 slice, map, chan 类型分配内存和初始化一个对象，make 也只能用在这3种类型上

```
a := make([]int, 2, 4)

b := make(map[string]int)

c := make(chan int, 10)
```

### 总结
- `new` 可以为所有的类型分配内存，并初始化为零值，返回指针
- `make` 只能为 slice, map, chan 分配内存，并初始化，返回的是类型

`new` 并不常用，一般使用短变量声明
```
a := new(int)
a = 1

等价于：

a := 1
```