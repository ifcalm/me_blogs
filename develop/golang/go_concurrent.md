## Golang 并发特性

### 进程和线程
进程是程序在操作系统中的一次执行过程，系统进行资源分配和调度的一个独立单位

线程是进程的一个执行实体，是 CPU 调度和分配的基本单位，它是比进程更小的能独立运行的基本单位

一个进程可以创建和撤销多个线程，同一个进程中的多个线程之间可以并发执行

### 并发和并行
多线程程序在一核的 CPU 上运行，就是并发

多线程程序在多核的 CPU 上运行，就是并行

### 线程和协程
协程有独立的栈空间，共享堆空间，调度由用户自己控制，类似于用户级线程

一个线程可以跑多个协程，协程是轻量级的线程

goroutine 是由官方实现的超级 线程池

并发主要是由切换时间片来实现 同时 运行。

goroutine 奉行通过通信来共享内存，而不是共享内存来通信。


### goroutine
Golang 在语言层面对并发编程提供了支持，一种类似于协程的模式，称作 goroutine 的机制

只需要在函数调用语句前面添加 go 关键字，就可以创建并发执行单元。

与之配套的 channel 类型，用以实现 "以通信来共享内存"的 CSP 模式

```
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		fmt.Println("Hello,World")
	}()
	time.Sleep(1 * time.Second)
}
```
调度器不能保证多个 goroutine 执行次序，且进程退出时不会等待它们结束。

默认情况下，进程启动后仅允许一个系统线程服务于 goroutine。

可以使用 环境变量或者标准函数库 `runtime.GOMAXPROCS` 修改，让调度器用多个线程实现多核并行，而不仅仅是并发

```
package main

import (
	"math"
	"sync"
)

func sum(id int) {
	var x int64
	for i := 0; i < math.MaxUint32; i++ {
		x += int64(i)
	}
	println(id, x)
}

func main() {
	wg := new(sync.WaitGroup)
	wg.Add(2)

	for i := 0; i < 2; i++ {
		go func(id int) {
			defer wg.Done()
			sum(id)
		}(i)
	}
	wg.Wait()
}
```

### chan
channel 是 CSP 模式的具体实现，用于多个 goroutine 通信

### WaitGroup
WaitGroup 用于线程同步，

等待一个系列执行完成后才会继续向下执行。

WaitGroup 能够一直等到所有的 goroutine 执行完成，并且阻塞主线程的执行，直到所有的 goroutine 执行完成

WaitGroup 总共有三个方法，`Add()`,`Done()`,`Wait()`
Add：添加或减少等待 goroutine 的数量

Done：相当于 Add(-1)

wait：执行阻塞，直到所有的WaitGroup数量变成0

```
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	wg := sync.WaitGroup{}

	for i := 0; i < 10; i++ {
		wg.Add(1)
		go calc(&wg, i)
	}

	wg.Wait()
	fmt.Println("all goroutine finsh")
}

func calc(w *sync.WaitGroup, i int) {
	fmt.Println("calc:", i)
	time.Sleep(time.Second)
	w.Done()
}
```

### Context
在Go中http包的Server中，每一个请求在都有一个对应的goroutine去处理。请求处理函数通常会启动额外的goroutine用来访问后端服务，比如数据库和RPC服务。用来处理一个请求的goroutine通常需要访问一些与请求特定的数据，比如终端用户的身份认证信息，验证相关的令牌，请求的截止时间。然后系统才能释放这些goroutine占用的资源。

在Google内部，开发了Context包，专门用来简化对于处理单个请求的多个goroutine之间与请求域的数据，取消信号，截止时间等相关操作，这些操作可能涉及多个API调用