### Linux 进程的内存模型

### 对 10 亿数据进行去重

### redis 的 ZSet 底层数据结构实现原理，跳跃表如何确定插入数据的层级

### HTTP 是如何实现协议的？头和体怎么区分

### TCP 进行连接的时候，linux 中需要实现多少种方法？关联的方法有哪些

### 谈谈微服务

### 服务治理中做了些什么

### HTTP 的状态有哪些分别代表什么？302 403 404 502

### 创建一个大内存，是堆还是栈


--------------------------------

### 链表有环的判断，写伪代码

### GPM 调度模型

### 浏览器的前进后退使用的数据结构

### TCP 的三次握手，第一次握手的 ack 包含哪些信息，什么时候会有 Time_Wait

### 栈保存的是什么，堆呢？地址生长方向是什么？为什么这么设计？

### Go 的栈大小是多少？最大值是多少？

### 两个链表像拉链一样相交，获取这些交点

### 后顺遍历树结构，如何释放树结构的内存信息

### Linux 的网络拥堵如何排查

### TCP 连接，服务端发现丢包之后是怎么处理的

### 贪心算法

### 队列如何保证先进先出，栈呢

### linux 权限控制是如何区分的


-----------------------------------------


### 谈一下Go的GC机制

### 说下三色标记算法的原理

### 判断链表是否有回环

### 介绍下自己的项目

### 开发的流程规范是什么

### 半连接是什么

### 粘包是什么？怎么发送的

### 怎么创建索引

### 怎么避免缓存击穿，还有其他的什么方法吗

### go的mutx怎么使用，乐观和悲观锁分别怎么实现，使用场景是什么

### 服务器受到攻击怎么定位服务器问题

### rpc的具体实现

### 怎么反转树的左右节点

### 谈谈epoll和select



-------------------------------------------

回答问题要有深度和广度，一个问题要由此及彼的回答，并且和多语言之间进行对比


---------------------------------------------


```
对于 func add(args ...int) int {} 调用方式正确的选项有（）
A. add(1, 2)
B. add(1, 3, 7)
C. add([]int{1, 2})
D. add([]int{1, 3, 7}…)
```

```
变量的初始化，下面正确的使用方式是（）
A. var i int = 10
B. var i = 10
C. i := 10
D. i = 10
```

```
golang 中的引用类型包括（）
A. string
B. map
C. channel
D. interface
```

```
关于整型切片的初始化，下面正确的是（）
A. s := make([]int)
B. s := make([]int, 0)
C. s := make([]int, 5, 10)
D. s := []int{1, 2, 3, 4, 5}
```

```
关于 channel，下面语法正确的是（）
A. var ch chan int
B. ch := make(chan int)
C. <- ch
D. ch <-
```

```
关于无缓冲和有缓冲的 channel，下面说法正确的是（）
A. 无缓冲的 channel 是默认的缓冲为 1 的 channel
B. 无缓冲的 channel 和有缓冲的 channel 都是同步的
C. 无缓冲的 channel 和有缓冲的 channel 都是非同步的
D. 无缓冲的 channel 是同步的，而有缓冲的 channel 是非同步的
```

### 简要描述下变量逃逸

变量逃逸就是变量的作用域的改变

- [Go变量逃逸](https://www.cnblogs.com/itbsl/p/10476674.html)



### 简要描述下 slice 在 append 时发生了什么

### 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标

### 给定两个数组，编写一个函数来计算它们的交集、以及差集


### 简单谈下 defer 的应用场景及注意事项

### 简单谈下 chan 的应用场景及注意事项

### 简述进程、线程、协程的含义及区别

### 往一个对象里写入 10w 条数据，怎样保证数据的准确性

chan、mutex 之类的胡扯就对了

### 简单介绍下 interface 的应用场景

### 线上 cpu 和内存突然飙高后应该怎么排错

### 哪些操作会导致内存泄漏

### 哪些操作会导致 io 开销大幅上升


-----------------------------------------------------

### `slice`, `map`, `channel` 函数传参形式

### gc 垃圾回收机制，三色标记法

### `channel` 的实现

### `context`

### 分布式事务

### `new` `make` 的区别，`make` 为什么不能返回指针

### 什么情况下go runtime 会创建一个协程

### `slice` 自动扩容，`map` 自动扩容

### `main` 函数 和 `init` 函数的区别

main 对应本包，init 对应所有包

### 什么是内存逃逸，什么情况下触发

### 多协程同步机制

- 锁
- `waitgroup`
- `channel`
- `context`

### 切片和数组的区别

### GMP 模型

### `defer`

### 协程负载高排查方法

### go 指针与 c指针 的区别

### map 找不到key 会怎样

### `channel` 线程安全吗，为什么

### `interface`

### 函数闭包

-----------------------------------------------------

### 什么是 goroutine，如何停止它

Go 协程是与其他函数同时运行的函数。 可以认为Go 协程是轻量级的线程，由Go 运行时来管理。 在函数调用前加上`go` 关键字，这次调用就会在一个新的goroutine 中并发执行。 当被调用的函数返回时，这个goroutine 也自动结束

- goroutine 是Go语言中并发的执行单位
- channel是Go语言中各个并发goroutine之前的通信机制

要创建 goroutine，在函数前加关键字 `go` 即可 `go f(x, y, z)`

可以通过向 Goroutine 发送一个信号通道来停止它
```
package main
func main() {
    quit := make(chan bool)
    go func() {
        for {
            select {
                case <-quit:
                return
                default:
                ...
            }
        }
    }()
    ...
    quit <-true
}
```

### 如何在运行时检查变量类型

**类型开关(Type Switch)**

类型开关(Type Switch)是在运行时检查变量类型的最佳方式。类型开关按类型而不是值来评估变量。每个 Switch 至少包含一个 case 用作条件语句，如果没有一个 case 为真，则执行 default


通常我们可能会有一连串的 `if … else` 结构来判断接口类型，以便于做出不同的决策
```
var e interface{}

if e == nil {
    ...
} else if v, ok := e.(int); ok {
    ...
} esle if v, ok := e.(float32); ok {
    ...
} else if v, ok := e.(string); ok {
    ...
} esle if v, ok := e.(bool); ok {
    ...
}
```

如果每次都这样写，相当费劲。Golang 关键字 `swtich` 支持一种非常便捷的写法，你可以这样:
```
var e interface{}

switch v := e.(type) {
    case nil:
        ...
    case int, uint, int32, uint32:
        ...
    case string:
        ...
    case bool:
        ...
    default:
        ...
}
```

注意上面的语法：`e.(type)` 只能用在 `switch` 关键字后面，它会返回 e 接口中的值部分

### Go 两个接口之间可以存在什么关系

如果两个接口有相同的方法列表，那么他们就是等价的，可以相互赋值。如果
接口 A 的方法列表是接口 B 的方法列表的子集，那么接口 B 可以赋值给接口A。接口查询是否成功，要在运行期才能够确定

### Go 当中同步锁有什么特点？作用是什么

Go 语言包中的 sync 包提供了两种锁类型：sync.Mutex 和 sync.RWMutex，前者是互斥锁，后者是读写锁

互斥锁是传统的并发程序对共享资源进行访问控制的主要手段，在 Go 中，似乎更推崇由 channel 来实现资源共享和通信

当一个 Goroutine（协程）获得了 Mutex 后，其他 Goroutine就只能乖乖的等待，除非该 Goroutine 释放了该 Mutex。RWMutex 在读锁占用的情况下，会阻止写，但不阻止读 RWMutex。 在写锁占用情况下，会阻止任何其他Goroutine（无论读和写）进来，整个锁相当于由该 Goroutine 独占同步锁的作用是保证资源在使用时的独有性，不会因为并发而导致数据错乱，保证系统的稳定性

建议：同一个互斥锁的成对锁定和解锁操作放在同一层次的代码块中。
使用锁的经典模式：
```
var lck sync.Mutex
func foo {
    lck.Lock()
    defer lck.Unlock()
    ...
}
```

`lck.Lock()` 会阻塞直到获取锁，然后利用 `defer` 语句在函数返回时自动释放锁

### go 读写锁

读写锁是分别针对读操作和写操作进行锁定和解锁操作的互斥锁。在 Go 语言中，读写锁由结构体类型 `sync.RWMutex` 代表

- 写锁定情况下，对读写锁进行读锁定或者写锁定，都将阻塞；而且读锁与写锁之间是互斥的

RWMutex 提供四个方法:
```
func (*RWMutex) Lock      //写锁
func (*RWMutex) Unlock    //写解锁
func (*RWMutex) RLock()   //读锁
func (*RWMutex) RUnlock() //读解锁
```

```
package main

import (
    "fmt"
    "sync"
    "time"
)

var m *sync.RWMutex
func main() {
    wg := sync.WaitGroup{}
    wg.Add(20)
    data := 0
    for i := 0; i < 10; i++ {
        go func(t int) {
            m.RLock()
            defer m.RUnlock()
            fmt.Println(data)
            wg.Done()
            time.Sleep(2 * time.Second)
        }(i)

        go func(t int) {
            m.Lock()
            defer m.Unlock()
            data += t
            fmt.Println(data, t)
            wg.Done()
            time.Sleep(2 * time.Second)
        }(i)
    }
    wg.Wait()
}
```

### Go 语言当中 Channel（通道）有什么特点，需要注意什么

- 如果从一个 nil 的 channel 中接收数据，会造成阻塞
- 给一个已经关闭的 channel 发送数据， 会引起 panic
- 从一个已经关闭的 channel 接收数据， 如果缓冲区中为空，则返回零值

### Go 语言当中 Channel 缓冲有什么特点

无缓冲的 channel 是同步的，而有缓冲的 channel 是非同步的

### Go 语言中 cap 函数可以作用于哪些内容

- array
- slice
- channel

### Go Convey 是什么？一般用来做什么

- go convey 是一个支持 Golang 的单元测试框架
- go convey 能够自动监控文件修改并启动测试，并可以将测试结果实时输出到 Web 界面
- go convey 提供了丰富的断言简化测试用例的编写

### Go 语言当中 new 的作用是什么

new 的作用是初始化一个内置类型的指针， new 函数是内建函数

- 使用 new 函数来分配空间
- 传递给 new 函数的是一个**类型**，而不是一个值
- 返回值是指向这个新分配的地址的指针

### Go 语言中 make 的作用是什么

make 的作用是为 slice, map, chan 进行初始化。
make 函数是内建函数

make()函数的目的和 new()不同, make仅仅用于创建 slice, map, channel。 而且返回类型是实例

### Printf()，Sprintf()，FprintF() 都是格式化输出，有什么不同

虽然这三个函数，都是格式化输出，但是输出的目标不一样

- Printf 是标准输出，一般是屏幕，也可以重定向
- Sprintf()是把格式化字符串输出到指定的字符串中
- Fprintf()是把格式化字符串输出到文件中

### Go 语言当中数组和切片的区别是什么

数组固定长度。数组长度是数组类型的一部分，所以`[3]int` 和`[4]int` 是两种不同的数组类型数组需要指定大小，不指定也会根据初始化，自动推算出大小，大小不可改变。数组是通过值传递的

切片可以改变长度。切片是轻量级的数据结构，三个属性，指针，长度，容量
不需要指定大小切片是地址传递可以通过数组来初始化，也可以通过内置函数 make()来初始化

### Go 语言当中值传递和地址传递如何运用？有什么区别

- 值传递只会把参数的值复制一份放进对应的函数，两个变量的地址不同，不可相互修改
- 地址传递会将变量地址传入对应的函数，在函数中可以对该变量进行值内容的修改

### Go 语言当中数组和切片在传递的时候的区别是什么

- 数组是值传递
- 切片是引用传递


### 