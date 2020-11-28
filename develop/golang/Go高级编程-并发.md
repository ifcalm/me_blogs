在早期，CPU都是以单核的形式顺序执行机器指令。Go语言的祖先C语言正是这种顺序编程语言的代表。顺序编程语言中的顺序是指：所有的指令都是以串行的方式执行，在相同的时刻有且仅有一个CPU在顺序执行程序的指令

随着处理器技术的发展，单核时代以提升处理器频率来提高运行效率的方式遇到了瓶颈，目前各种主流的CPU频率基本被锁定在了3 GHz附近。单核CPU发展的停滞，为多核CPU的发展带来了机遇。相应地，编程语言也开始逐步向并行化的方向发展。Go语言正是在多核和网络化的时代背景下诞生的原生支持并发的编程语言

常见的并行编程有多种模型，主要有多线程、消息传递等

从理论上来看，多线程和基于消息的并发编程是等价的

由于多线程并发模型可以自然对应到多核的处理器，主流的操作系统因此也都提供了系统级的多线程支持，同时从概念上讲多线程似乎也更直观，因此多线程编程模型逐步被吸纳到主流的编程语言特性或语言扩展库中

而主流编程语言对基于消息的并发编程模型支持则相对较少，Erlang语言是支持基于消息传递并发编程模型的代表者，它的并发体之间不共享内存

Go语言是基于消息并发模型的集大成者，它将基于CSP模型的并发编程内置到了语言中，通过一个go关键字就可以轻易地启动一个Goroutine，与Erlang不同的是，Go语言的Goroutine之间是共享内存的

## Go 并发

### Goroutine和系统线程
`Goroutine`是Go语言特有的并发体，是一种轻量级的线程，由`go`关键字启动。在真实的Go语言的实现中，`Goroutine`和系统线程也不是等价的。尽管两者的区别实际上只是一个量的区别，但正是这个量变引发了Go语言并发编程质的飞跃

首先，每个系统级线程都会有一个固定大小的栈（一般默认可能是2MB），这个栈主要用来保存函数递归调用时的参数和局部变量

固定了栈的大小导致了两个问题:
- 对于很多只需要很小的栈空间的线程是一个巨大的浪费
- 对于少数需要巨大栈空间的线程又面临栈溢出的风险

针对这两个问题的解决方案是：要么降低固定的栈大小，提升空间的利用率；要么增大栈的大小以允许更深的函数递归调用，但这两者是无法兼得的

一个`Goroutine`会以一个很小的栈启动（可能是2KB或4KB），当遇到深度递归导致当前栈空间不足时，`Goroutine`会根据需要动态地伸缩栈的大小（主流实现中栈的最大值可达到1GB）。因为启动的代价很小，所以我们可以轻易地启动成千上万个`Goroutine`

Go的运行时还包含了其自己的调度器，这个调度器使用了一些技术手段，可以在`n`个`操作系统线程`上多工调度`m`个`Goroutine`

Go调度器的工作原理和内核的调度是相似的，但是这个调度器只关注单独的Go程序中的`Goroutine`

`Goroutine`采用的是半抢占式的协作调度，只有在当前`Goroutine`发生阻塞时才会导致调度；同时发生在用户态，调度器会根据具体函数只保存必要的寄存器，切换的代价要比系统线程低得多

运行时有一个`runtime.GOMAXPROCS`变量，用于控制当前运行正常非阻塞`Goroutine`的系统线程数目

在Go语言中启动一个`Goroutine`不仅和调用函数一样简单，而且`Goroutine`之间调度代价也很低，这些因素极大地促进了并发编程的流行和发展

### 原子操作
所谓的原子操作就是并发编程中`最小的且不可并行化`的操作

通常，如果多个并发体对同一个共享资源进行的操作是原子的话，那么同一时刻最多只能有一个并发体对该资源进行操作

从线程角度看，在当前线程修改共享资源期间，其他线程是不能访问该资源的

原子操作对多线程并发编程模型来说，不会发生有别于单线程的意外情况，共享资源的完整性可以得到保证

一般情况下，原子操作都是通过`互斥`访问来保证的，通常由特殊的CPU指令提供保护。当然，如果仅仅是想模拟粗粒度的原子操作，可以借助于`sync.Mutex`来实现:
```
package main

import (
	"fmt"
	"sync"
)

var total struct {
	sync.Mutex
	value int
}

func worker(wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 0; i <= 10000; i++ {
		total.Lock()
		total.value += i
		total.Unlock()
	}
}

func main() {
	var wg sync.WaitGroup
	wg.Add(2)
	go worker(&wg)
	go worker(&wg)
	wg.Wait()
	fmt.Println(total.value)
}
```
在worker的循环中，为了保证`total.value += i`的原子性，我们通过`sync.Mutex`加锁和解锁来保证该语句在同一时刻只被一个线程访问。对多线程模型的程序而言，进出临界区前后进行加锁和解锁都是必需的。如果没有锁的保护，total的最终值将由于多线程之间的竞争而可能不正确

用互斥锁来保护一个数值型的共享资源麻烦且效率低下。标准库的`sync/atomic`包对原子操作提供了丰富的支持。我们可以重新实现上面的例子:
```
package main

import (
	"sync"
	"sync/atomic"
)

var total uint64

func worker(wg *sync.WaitGroup) {
	defer wg.Done()
	var i uint64
	for i = 0; i <= 100; i++ {
		atomic.AddUint64(&total, i)
	}
}

func main() {
	var wg sync.WaitGroup
	wg.Add(2)
	go worker(&wg)
	go worker(&wg)
	wg.Wait()
}
```

`atomic.AddUint64 ()`函数调用保证了total的读取、更新和保存是一个原子操作，因此在多线程中访问也是安全的

原子操作配合互斥锁可以实现非常高效的单件模式。互斥锁的代价比普通整数的原子读写高很多，在性能敏感的地方可以增加一个数字型的标志位，通过原子检测标志位状态降低互斥锁的使用次数来提高性能

```
type singleton struct{}

var (
	instance    *singleton
	initialized uint32
	mu          sync.Mutex
)

func Instance() *singleton {
	if atomic.LoadUint32(&initialized) == 1 {
		return instance
	}
	mu.Lock()
	defer mu.Unlock()
	if instance == nil {
		defer atomic.StoreUint32(&initialized, 1)
		instance = &singleton{}
	}
	return instance
}
```

我们将通用的代码提取出来，就成了标准库中sync.Once的实现:
```
type Once struct {
    m    Mutex
    done unit32
}

func (o *Once) Do(f func()) {
    if atomic.LoadUint32(&o.done) == 1 {
        return
    }
    o.m.Lock()
    defer o.m.Unlock()
    if o.done == 0 {
        defer atomic.StoreUint32(&o.done, 1)
        f()
    }
}
```

基于`sync.Once`重新实现单件（singleton）模式:
```
var (
    instance *singleton
    once     sync.Once
)

func Instance() *singleton {
    once.Do(func() {
        instance = &singleton{}
    })
    return instance
}
```

`sync/atomic`包对基本数值类型及复杂对象的读写都提供了原子操作的支持。`atomic.Value`原子对象提供了`Load()`和`Store()`两个原子方法，分别用于加载和保存数据，返回值和参数都是`interface{}`类型，因此可以用于任意的自定义复杂类型

```
var config atomic.Value  //保存当前配置信息
config.Store(loadConfig())
//启动一个后台线程，加载更新后的配置信息
go func() {
    for {
        time.Sleep(time.Second)
        config.Store(loadConfig())
    }
}()

//用于处理请求的工作者线程始终采用最新的配置信息
for i := 0; i < 10; i++ {
    go func() {
        for r := range requests() {
            c := config.Load()
            //...
        }
    }()
}
```

这是一个简化的生产者消费者模型：后台线程生成最新的配置信息；前台多个工作者线程获取最新的配置信息。所有线程共享配置信息资源

### 顺序一致性内存模型
如果只是想简单地在线程之间进行数据同步的话，`原子操作`已经为编程人员提供了一些同步保障。不过这种保障有一个前提：`顺序一致性的内存模型`。要了解顺序一致性，先看一个简单的例子:
```
var a string
var done bool

func setup() {
    a = "hello, world"
    done = true
}

func main() {
    go setup()
    for != done {}
    fmt.Println(a)
}
```
我们创建了setup线程，用于对字符串a的初始化工作，初始化完成之后设置done标志为true。main()函数所在的主线程中，通过for !done {}检测done变为true时，认为字符串初始化工作完成，然后进行字符串的打印工作

但是Go语言并不保证在`main()`函数中观测到的对`done`的写入操作发生在对字符串a的写入操作之后，因此程序很可能打印一个空字符串。更糟糕的是，因为两个线程之间没有同步事件，`setup线程`对`done`的写入操作甚至无法被`main线程`看到，main()`函数有可能陷入死循环中

在Go语言中，同一个`Goroutine线程`内部，顺序一致性的内存模型是得到保证的。但是不同的`Goroutine`之间，并不满足顺序一致性的内存模型，需要通过明确定义的同步事件来作为同步的参考。如果两个事件不可排序，那么就说这两个事件是并发的。为了最大化并行，Go语言的编译器和处理器在不影响上述规定的前提下可能会对执行语句重新排序（CPU也会对一些指令进行乱序执行）

因此，如果在一个`Goroutine`中顺序执行`a=1; b=2;`这两个语句，虽然在当前的`Goroutine`中可以认为`a=1;`语句先于`b=2;`语句执行，但是在另一个`Goroutine`中`b=2;`语句可能会先于`a=1;`语句执行，甚至在另一个`Goroutine`中无法看到它们的变化（可能始终在寄存器中）。也就是说在另一个`Goroutine`看来`a=1; b=2;`这两个语句的执行顺序是不确定的。如果一个并发程序无法确定事件的顺序关系，那么程序的运行结果往往会有不确定的结果
```
func main() {
    go fmt.Println("hello, go")
}
```

根据Go语言规范，`main()`函数退出时程序结束，不会等待任何后台线程。因为`Goroutine`的执行和`main()`函数的返回事件是并发的，谁都有可能先发生，所以什么时候打印、能否打印都是未知的

用前面的原子操作并不能解决问题，因为我们无法确定两个原子操作之间的顺序。解决问题的办法就是通过`同步原语`来给两个事件明确排序:
```
func main() {
    done := make(chan int)
    go func() {
        fmt.Println("hello, go")
        done <- 1
    }()
    <-done
}
```
当`<-done`执行时，必然要求`done <- 1`也已经执行。根据同一个`Goroutine`依然满足顺序一致性规则，可以判断当`done <- 1`执行时，`fmt.Println("hello, go")`语句必然已经执行完成了。因此，现在的程序确保可以正常打印结果

当然，通过`sync.Mutex`互斥量也是可以实现同步的:
```
func main() {
    var mu sync.Mutex
    mu.Lock()
    go func() {
        fmt.Println("hello, go")
        mu.Unlock()
    }()
    mu.Lock()
}
```
可以确定，后台线程的`mu.Unlock()`必然在`fmt.Println("hello, go")`完成后发生（同一个线程满足顺序一致性），`main()`函数的第二个`mu.Lock()`必然在后台线程的`mu.Unlock()`之后发生（sync.Mutex保证），此时后台线程的打印工作已经顺利完成了

### 初始化顺序
Go程序的初始化和执行总是从`main.main()`函数开始的。但是如果`main包`里导入了其他的包，则会按照顺序将它们包含到`main包`里,这里的导入顺序依赖具体实现，一般可能是以文件名或包路径名的字符串顺序导入

如果某个包被多次导入，那么在执行的时候只会导入一次

当一个包被导入时，如果它还导入了其他的包，则先将其他的包包含进来，然后创建和初始化这个包的常量和变量。再调用包里的`init()`函数，如果一个包有多个`init()`函数，实现可能是以文件名的顺序调用，那么同一个文件内的多个`init()`是以出现的顺序依次调用的

`init()`不是普通函数，可以定义多个，但是不能被其他函数调用

最终，在`main`包的所有包常量、包变量被创建和初始化，并且只有在`init()`函数被执行后，才会进入`main.main()`函数，程序开始正常执行

要注意的是，在`main.main()`函数执行之前所有代码都运行在同一个`Goroutine`中，也是运行在程序的主系统线程中。如果某个`init()`函数内部用`go`关键字启动了新的`Goroutine`，那么新的`Goroutine`和`main.main()`函数是并发执行的

因为所有的`init()`函数和`main()`函数都是在主线程完成，它们也是满足顺序一致性模型的

### Goroutine 的创建
`go`语句会在当前`Goroutine`对应函数返回前创建新的`Goroutine`
```
var a string
func f() {
    fmt.Println(a)
}

func hello() {
    a = "hello, go"
    go f()
}
```
执行`go f()`语句创建`Goroutine`和`hello()`函数是在同一个`Goroutine`中执行，根据语句的书写顺序可以确定`Goroutine`的创建发生在`hello()`函数返回之前，但是新创建`Goroutine`对应的`f()`的执行事件和`hello()`函数返回的事件则是不可排序的，也就是`并发`的。调用`hello()`可能会在将来的某一时刻打印`hello, go`，也很可能是在`hello()`函数执行完成后才打印

### 基于通道的通信
通道（channel）是在Goroutine之间进行同步的主要方法

在无缓存的通道上的每一次发送操作都有与其对应的接收操作相匹配，发送和接收操作通常发生在不同的`Goroutine`上,**在同一个Goroutine上执行两个操作很容易导致死锁**。无缓存的通道上的发送操作总在对应的接收操作完成前发生

```
var done = make(chan bool)
var msg string

func aGoroutine() {
    msg = "hello, go"
    done <- true
}

func main() {
    go aGoroutine()
    <-done
    fmt.Println(msg)
}
```
可保证打印出`hello, go`。该程序首先对msg进行写入，然后在done通道上发送同步信号，随后从`done`接收对应的同步信号，最后执行Println()函数

若在关闭通道后继续从中接收数据，接收者就会收到该通道返回的零值。因此在这个例子中，用`close(done)`关闭通道代替`done <- true`依然能保证该程序产生相同的行为

```
var done = make(chan bool)
var msg string

func aGoroutine() {
    msg = "hello, go"
    close(done)
}

func main() {
    go aGoroutine()
    <-done
    fmt.Println(msg)
}
```

对于从无缓存通道进行的接收，发生在对该通道进行的发送完成之前

基于上面这个规则可知，交换两个`Goroutine`中的接收和发送操作也是可以的,但是很危险
```
var done = make(chan bool)
var msg string

func aGoroutine() {
    msg = "hello, go"
    <-done
}

func main() {
    go aGoroutine()
    done <-true
    fmt.Println(msg)
}
```

对于带缓存的通道，对于通道中的第`K`个接收完成操作发生在第`K+C`个发送操作完成之前，其中`C`是管道的缓存大小。如果将`C`设置为0自然就对应无缓存的通道，也就是第`K`个接收完成在第`K`个发送完成之前。因为无缓存的通道只能同步发1个，所以也就简化为前面无缓存通道的规则：对于从无缓存通道进行的接收，发生在对该通道进行的发送完成之前

我们可以根据控制通道的缓存大小来控制并发执行的Goroutine的最大数目:
```
var limit = make(chan int, 3)
func main() {
    for _, w := range work {
        go func() {
            limit <- 1
            w()
            <-limit
        }()
    }
    select{}
}
```
最后一句`select{}`是一个空的通道选择语句，该语句会导致`main线程`阻塞，从而避免程序过早退出。还有`for{}、<-make(chan int)`等诸多方法可以达到类似的效果。因为main线程被阻塞了，如果需要程序正常退出的话，可以通过调用`os.Exit(0)`实现

### 不靠谱的同步
严谨的并发程序的正确性不应该是依赖于CPU的执行速度和休眠时间等不靠谱的因素的

严谨的并发也应该是可以静态推导出结果的：根据线程内顺序一致性，结合通道或`sync`事件的可排序性来推导，最终完成各个线程各段代码的偏序关系排序。如果两个事件无法根据此规则来排序，那么它们就是并发的，也就是执行先后顺序不可靠的

**解决同步问题的思路是相同的：使用显式的同步**

## 常见的并发模式

Go语言最吸引人的地方是它内建的并发支持

并发不是并行。并发更关注的是程序的设计层面，并发的程序完全是可以顺序执行的，只有在真正的多核CPU上才可能真正地同时运行。并行更关注的是程序的运行层面，并行一般是简单的大量重复

为了更好地编写并发程序，从设计之初Go语言就注重如何在编程语言层级上设计一个简洁安全高效的抽象模型，让程序员专注于分解问题和组合方案，而且不用被线程管理和信号互斥这些烦琐的操作分散精力

在并发编程中，对共享资源的正确访问需要精确地控制，在目前的绝大多数语言中，都是通过加锁等线程同步方案来解决这一困难问题

而Go语言却另辟蹊径，它将共享的值通过通道传递

在任意给定的时刻，最好只有一个Goroutine能够拥有该资源

Go语言将其并发编程哲学化为一句口号：`不要通过共享内存来通信，而应通过通信来共享内存`

### 并发版本的`Hello, World`
发编程的核心概念是同步通信，但是同步的方式却有多种。先以大家熟悉的互斥量`sync.Mutex`来实现同步通信

```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	go func() {
		fmt.Println("Hello, World")
		mu.Lock()
	}()
	mu.Unlock()
}
```
根据文档，我们不能直接对一个未加锁状态的`sync.Mutex`进行解锁，这会导致运行时异常

因为`mu.Lock()`和`mu.Unlock()`并不在同一个`Goroutine`中，所以也就不满足顺序一致性内存模型。同时它们也没有其他的同步事件可以参考，这两个事件不可排序也就是可以并发的。因为可能是并发的事件，所以`main()`函数中的`mu.Unlock()`很有可能先发生，而这个时刻mu互斥对象还处于未加锁的状态，因而会导致运行时异常

下面是修复后的代码:
```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	mu.Lock()
	go func() {
		fmt.Println("Hello, World")
		mu.Unlock()
	}()
	mu.Lock()
}
```

修复的方式是在`main()`函数所在线程中执行两次`mu.Lock()`，当第二次加锁时会因为锁已经被占用（不是递归锁）而阻塞，`main()`函数的阻塞状态驱动后台线程继续向前执行。当后台线程执行到`mu.Unlock()`时解锁，此时打印工作已经完成了，解锁会导致`main()`函数中的第二个`mu.Lock()`阻塞状态取消，此时后台线程和主线程再没有其他的同步事件参考，它们退出的事件将是并发的：在`main()`函数退出导致程序退出时，后台线程可能已经退出了，也可能没有退出。虽然无法确定两个线程退出的时间，但是打印工作是可以正确完成的

使用`sync.Mutex`互斥锁同步是比较低级的做法。我们现在改用无缓存通道来实现同步:
```
package main

import (
	"fmt"
)

func main() {
	done := make(chan int)
	go func() {
		fmt.Println("Hello, World")
		<-done
	}()
	done <- 1
}
```

根据Go语言内存模型规范，对于从无缓存通道进行的接收，发生在对该通道进行的发送完成之前。因此，后台线程`<-done`接收操作完成之后，main线程的done<- 1发送操作才可能完成（从而退出main、退出程序），而此时打印工作已经完成了

上面的代码虽然可以正确同步，但是对通道的缓存大小太敏感：如果通道有缓存，就无法保证`main()`函数退出之前后台线程能正常打印了。更好的做法是将通道的发送和接收方向调换一下，这样可以避免同步事件受通道缓存大小的影响
```
package main

import (
	"fmt"
)

func main() {
	done := make(chan int, 1)
	go func() {
		fmt.Println("Hello, World")
		done <- 1
	}()
	<-done
}
```

对于带缓存的通道，对通道的第`K`个接收完成操作发生在第`K+C`个发送操作完成之前，其中`C`是通道的缓存大小。虽然通道是带缓存的，但是`main`线程接收完成是在后台线程发送开始但还未完成的时刻，此时打印工作也是已经完成的

基于带缓存通道，我们可以很容易将打印线程扩展到`N`个。下面的例子是开启10个后台线程分别打印:
```
package main

import (
	"fmt"
)

func main() {
	done := make(chan int, 10)
	for i := 0; i < cap(done); i++ {
		go func() {
			fmt.Println("Hello, World")
			done <- i
		}()
		<-done
	}
}
```

对于这种要等待`N`个线程完成后再进行下一步的同步操作有一个简单的做法，就是使用`sync.WaitGroup`来等待一组事件:
```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func() {
			fmt.Println("Hello, World")
			wg.Done()
		}()
	}
	wg.Wait()
}
```

其中`wg.Add(1)`用于增加等待事件的个数，必须确保在后台线程启动之前执行（如果放到后台线程之中执行则不能保证被正常执行到）。当后台线程完成打印工作之后，调用`wg.Done()`表示完成一个事件。`main()`函数的`wg.Wait()`是等待全部的事件完成

### 生产者/消费者模型

并发编程中最常见的例子就是生产者/消费者模型，该模型主要通过平衡生产线程和消费线程的工作能力来提高程序的整体处理数据的速度。简单地说，就是生产者生产一些数据，然后放到成果队列中，同时消费者从成果队列中来取这些数据。这样就让生产和消费变成了异步的两个过程。当成果队列中没有数据时，消费者就进入饥饿的等待中；而当成果队列中数据已满时，生产者则面临因产品积压导致CPU被剥夺的问题

```
package main

import (
	"fmt"
	"time"
)

//生产者
func Producer(factor int, out chan<- int) {
	for i := 0; ; i++ {
		out <- i * factor
	}
}

//消费者
func Consumer(in <-chan int) {
	for v := range in {
		fmt.Println(v)
	}
}

func main() {
	ch := make(chan int, 64)
	go Producer(3, ch)
	go Producer(5, ch)
	go Consumer(ch)

	time.Sleep(5 * time.Second)
}
```

我们开启了两个`Producer`生产流水线，分别用于生成3和5的倍数的序列。然后开启一个`Consumer`消费者线程，打印获取的结果。我们通过在`main()`函数休眠一定的时间来让生产者和消费者工作一定时间

我们可以让`main()`函数保存阻塞状态不退出，只有当用户输入`Ctrl+C`时才真正退出程序:
```
package main

import (
	"fmt"
	"os"
	"os/signal"
	"syscall"
)

//生产者
func Producer(factor int, out chan<- int) {
	for i := 0; ; i++ {
		out <- i * factor
	}
}

//消费者
func Consumer(in <-chan int) {
	for v := range in {
		fmt.Println(v)
	}
}

func main() {
	ch := make(chan int, 64)
	go Producer(3, ch)
	go Producer(5, ch)
	go Consumer(ch)

	//Ctrl+C 退出
	sig := make(chan os.Signal, 1)
	signal.Notify(sig, syscall.SIGINT, syscall.SIGTERM)
	fmt.Printf("quit (%v)\v", <-sig)
}
```

这个例子中有两个生产者，并且两个生产者之间无同步事件可参考，它们是并发的。因此，消费者输出的结果序列的顺序是不确定的，这并没有问题，生产者和消费者依然可以相互配合工作

### 发布/订阅模型
发布/订阅`（publish-subscribe）`模型通常被简写为`pub/sub模型`。在这个模型中，消息生产者成为发布者`（publisher）`，而消息消费者则成为订阅者（subscriber），生产者和消费者是`M :N`的关系。在传统生产者/消费者模型中，是将消息发送到一个队列中，而发布/订阅模型则是将消息发布给一个主题

```
待补充
```

### 控制并发数
有时候我们需要适当地控制并发的程度，因为这样不仅可给其他的应用/任务让出/预留一定的CPU资源，也可以适当降低功耗缓解电池的压力

在Go语言自带的`godoc`程序实现中有一个`vfs`的包对应虚拟的文件系统，在`vfs`包下面有一个`gatefs`的子包，`gatefs`子包的目的就是为了控制访问该虚拟文件系统的最大并发数。`gatefs`包的应用很简单:
```
import (
    "golang.org/x/tools/godoc/vfs"
    "golang.org/x/tools/godoc/vfs/gatefs"
)

func main() {
    fs := gatefs.New(vfs.OS("/path"), make(chan bool, 8))
    //...
}
```

其中`vfs.OS("/path")`基于本地文件系统构造一个虚拟的文件系统，然后`gatefs.New`基于现有的虚拟文件系统构造一个并发受控的虚拟文件系统

通过带缓存通道的发送和接收规则来实现最大并发阻塞:
```
var limit = make(chan int, 3)

func main() {
	for _, w := range work {
		go func() {
			limit <- 1
			w()
			<-limit
		}()
	}
	select {}
}
```

### 赢者为王
采用并发编程的动机有很多：并发编程可以简化问题，例如一类问题对应一个处理线程会更简单；并发编程还可以提升性能，在一个多核CPU上开两个线程一般会比开一个线程快一些。其实对提升性能而言，并不是程序运行速度快就表示用户体验好，很多时候程序能快速响应用户请求才是最重要的，当没有用户请求需要处理的时候才合适处理一些低优先级的后台任务

假设我们想快速地搜索`golang`相关的主题，我们可能会同时打开必应、谷歌或百度等多个检索引擎。当某个搜索最先返回结果后，就可以关闭其他搜索页面了。因为受网络环境和搜索引擎算法的影响，某些搜索引擎可能很快返回搜索结果，某些搜索引擎也可能等到他们公司倒闭也没有完成搜索。我们可以采用类似的策略来编写这个程序:

```
func main() {
    ch := make(chan string, 32)
    go func() {
        ch <- searchByBing("golang")
    }()
    go func() {
        ch <- searchByGoogle("golang")
    }()
    go func() {
        ch <- searchByBaidu("golang")
    }()
    fmt.Println(<-ch)
}
```

首先，创建了一个带缓存通道，通道的缓存数目要足够大，保证不会因为缓存的容量引起不必要的阻塞。然后开启了多个后台线程，分别向不同的搜索引擎提交搜索请求。当任意一个搜索引擎最先有结果之后，都会马上将结果发到通道中（因为通道带了足够的缓存，这个过程不会阻塞）。但是最终只从通道取第一个结果，也就是最先返回的结果

通过适当开启一些冗余的线程，尝试用不同途径去解决同样的问题，最终以赢者为王的方式提升了程序的相应性能

### 并发的安全退出
有时候需要通知`Goroutine`停止它正在干的事情，特别是当它工作在错误的方向上的时候。Go语言并没有提供一个直接终止`Goroutine`的方法，因为这样会导致`Goroutine`之间的共享变量处在未定义的状态上。但是如果想要退出两个或者任意多个`Goroutine`怎么办呢

Go语言中不同`Goroutine`之间主要依靠通道进行通信和同步。要同时处理多个通道的发送或接收操作，需要使用`select`关键字,当`select`有多个分支时，会随机选择一个可用的通道分支，如果没有可用的通道分支，则选择`default`分支，否则会一直保持阻塞状态

基于`select`实现的通道的超时判断:
```
select {
    case v := <-in:
        fmt.Println(v)
    case <-time.After(time.Second):
        return
}
```

通过`select`的`default`分支实现非阻塞的通道发送或接收操作:
```
select {
    case v := <-in:
        fmt.Println(v)
    default:
    //...
}
```

通过`select`来阻止`main()`函数退出:
```
func main() {
    //...
    select {}
}
```

当有多个通道均可操作时，`select`会随机选择一个通道。基于该特性我们可以用`select`实现一个生成随机数序列的程序:
```
func main() {
    ch := make(chan int)
    go func() {
        for {
            select {
                case ch <- 0:
                case ch <- 1:
            }
        }
    }()
    for v := range ch {
        fmt.Println(v)
    }
}
```

我们通过`select`和`default`分支可以很容易实现一个`Goroutine`的退出控制:
```
func worker(channel chan bool) {
    for {
        select {
            default:
                fmt.Println("hello")
                //正常工作
            case <- channel:
                //退出
        }
    }
}

func main() {
    channel := make(chan bool)
    go worker(channel)
    time.Sleep(time.Second)
    channel <- true
}
```

但是通道的发送操作和接收操作是一一对应的，如果要停止多个`Goroutine`，那么可能需要创建同样数量的通道，这个代价太大了。其实我们可以通过`close()`关闭一个通道来实现广播的效果，所有从关闭通道接收的操作均会收到一个零值和一个可选的失败标志

```
func worker(channel chan bool) {
    for {
        default:
            fmt.Println("hello")
            //正常工作
        case <- channel:
        //退出
    }
}

func main() {
    channel := make(chan bool)
    for i := 0; i < 10; i++ {
        go worker(channel)
    }
    time.Sleep(time.Second)
    close(channel)
}
```

我们通过`close()`来关闭`channel`通道，向多个`Goroutine`广播退出的指令。不过这个程序依然不够稳健：当每个`Goroutine`收到退出指令退出时一般会进行一定的清理工作，但是退出的清理工作并不能保证被完成，因为`main`线程并没有等待各个工作`Goroutine`退出工作完成的机制。我们可以结合`sync.WaitGroup`来改进

```
func worker(wg *sync.WaitGroup, channel chan bool) {
    defer wg.Done()
    for {
        select {
            default:
                fmt.Println("hello")
            case <-channel:
                return
        }
    }
}

func main() {
    channel := make(chan bool)
    var wg sync.WaitGroup
    for i := 0; i < 10; i++ {
        wg.Add(1)
        go worker(&wg, channel)
    }
    time.Sleep(time.Second)
    close(channel)
    wg.Wait()
}
```

现在每个工作者并发体的创建、运行、暂停和退出都是在`main()`函数的安全控制之下了

### context包

在`Go 1.7`发布时，标准库增加了一个`context`包，用来简化对于处理单个请求的多个`Goroutine`之间与请求域的数据、超时和退出等操作，官方有博客文章对此做了专门介绍。我们可以用`context`包来重新实现前面的线程安全退出或超时的控制

```
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)

func worker(ctx context.Context, wg *sync.WaitGroup) error {
	defer wg.Done()
	for {
		select {
		default:
			fmt.Println("hello")
		case <-ctx.Done():
			return ctx.Err()
		}
	}
}

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	var wg sync.WaitGroup
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go worker(ctx, &wg)
	}
	time.Sleep(time.Second)
	cancel()
	wg.Wait()
}
```

当并发体超时或`main`主动停止工作者`Goroutine`时，每个工作者都可以安全退出

并发是一个非常大的主题，这里只展示几个非常基础的并发编程的例子。官方文档也有很多关于并发编程的讨论，国内也有专门讨论Go语言并发编程的书籍。读者可以根据自己的需求查阅相关的文献