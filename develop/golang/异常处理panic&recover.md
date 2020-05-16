## 异常处理

编程语言一般都会有异常捕获机制

### 触发panic
只需要调用`panic` 函数，就可以触发宕机

```
package main

func main() {
	panic("test")
}
```
运行后，报错:
```
panic: test

goroutine 1 [running]:
main.main()
        C:/xxx/main.go:4 +0x40
exit status 2
```

### 捕获 panic
发生异常，有时候需要捕获，Go 内建函数 `recover` 可以让程序在发送宕机后起死回生。

但是 `recover` 的使用，必须在 `defer` 函数中才能生效，其他作用域下，它是不工作的。

```
package main

import "fmt"

func main() {
	testA(20)

    //如果执行到这句，说明panic被捕获了
    //后续的程序能继续运行
	fmt.Println("It's ok")
}

func testA(x int) {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()

    //制造数组越界，触发 panic
	var arr [10]int
	arr[x] = 88
}
```

代码执行结果如下：
```
runtime error: index out of range
It's ok
```

`recover()` 可以将捕获到的 `panic` 信息打印

通常来说，不应该对进入panic宕机的程序做任何处理，但有时，需要我们从宕机中恢复，至少我们可以在程序崩溃前做一些操作，比如关闭所有连接，释放资源。

### defer无法跨协程
即使 panic 会导致整个程序退出，但在退出前，若有 defer 延迟函数，还是得执行完 defer

但是这个 defer 在多个协程之间是没有效果的，在子协程里触发 panic，只能触发自己协程内的 defer，而不能调用 main 协程里的 defer 函数。
```
package main

import (
	"fmt"
	"time"
)

func main() {
	defer fmt.Println("in main")
	go func() {
		defer println("in goroutine")
		panic("test")
	}()

	time.Sleep(2 * time.Second)
}
```
输出结果为：
```
in goroutine
panic: test

goroutine 20 [running]:
main.main.func1()
        C:/xxx/main.go:12 +0x71
created by main.main
        C:/xxx/main.go:10 +0x98
exit status 2
```
### 总结
Go 的异常抛出与捕获，依赖两个内置函数：
- `panic` : 抛出异常，是程序奔溃
- `recover` : 捕获异常，恢复程序或做收尾工作