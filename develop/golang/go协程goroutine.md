## 协程（goroutine）
一个 goroutine 本身就是一个函数，当你直接调用时，它就是一个普通函数，如果你在调用前加一个关键字 `go`，那就开了一个 goroutine

```
func()      //执行一个函数
go func()   //开启一个协程执行这个函数
```

一个go程序的入口通常是`main`函数，程序启动后，main 函数最先运行，称为`main goroutine`。

`go func()` 启动协程需在 main 中或者其下调用的代码中。

main 相当于主线程，当 main 函数执行完成后，这个线程也就终结了，其下运行的所有协程不管是不是运行中，都得退出。

```
package main

import "fmt"

func main() {
	go testA()
	fmt.Println("hello, world")
}

func testA() {
	fmt.Println("hello, go")
}
```
上面代码只会输出 `hello, world`，不会输出 `hello, go`，因为协程创建需要时间，当 `hello, world`打印后，协程还没来得及执行，主线程就结束了，其下的协程也必须退出。

当然我们可以使用 `time.Sleep` 来使 main 阻塞，使协程能够运行完成。
```
package main

import (
	"fmt"
	"time"
)

func main() {
	go testA()
	fmt.Println("hello, world")
	time.Sleep(time.Second)
}

func testA() {
	fmt.Println("hello, go")
}
```

### 多协程
```
package main

import (
	"fmt"
	"time"
)

func main() {
	go testA("one")
	go testA("two")
	time.Sleep(time.Second)
}

func testA(name string) {
	for i := 0; i < 10; i++ {
		fmt.Printf("goroutine %s\n", name)
		time.Sleep(10 * time.Millisecond)
	}
}
```
输出结果如下所示:
```
goroutine one
goroutine two
goroutine two
goroutine one
goroutine one
goroutine two
goroutine one
goroutine two
goroutine two
goroutine one
goroutine one
goroutine two
goroutine one
goroutine two
goroutine one
goroutine two
goroutine two
goroutine one
goroutine two
goroutine one
```
可以看到两个协程并发执行。