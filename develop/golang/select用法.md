## select - case
select - case 用法比较单一，仅能用于信道的相关操作。

```
package main

import "fmt"

func main() {
	c1 := make(chan string, 1)
	c2 := make(chan string, 1)
	
	c2 <- "hello"
	c1 <- "world"

	select {
	case msg1 := <-c1:
		fmt.Println("c1 received: ", msg1)
	case msg2 := <-c2:
		fmt.Println("c2 received: ", msg2)
	default:
		fmt.Println("No data received.")
	}
}
```

在运行 `select` 时，如果有机会，会运行所有的 `case`，只要有一个 信道进行了操作，`select`就结束

### 避免造成死锁
`select` 在运行过程中，必须命中其中的某一分支，如果遍历完所有的`case`后，没有命中`case`，即没有可以执行的信道操作语句，就会进入`default`语句。

但是如果你没有写default分支，`select`就会阻塞，直到命中某个`case`，如果一直没命中，`select`就会抛出 `deadlock` 错误

```
package main

import "fmt"

func main() {
	c1 := make(chan string, 1)
	c2 := make(chan string, 1)

	select {
	case msg1 := <-c1:
		fmt.Println("c1 received: ", msg1)
	case msg2 := <-c2:
		fmt.Println("c2 received: ", msg2)
	}
}
```
运行代码后，报如下错误：
```
fatal error: all goroutines are asleep - deadlock!
```

#### 解决如上死锁的两种方法
- 在写 `select` 的时候，也写好 `default` 分支代码
- 让其中的某个信道可以接收到数据，即可以执行到某个`case`

### select的随机性
`switch - case` 里面的 `case` 是顺序执行，但在`select` 里不是

```
package main

import "fmt"

func main() {
	c1 := make(chan string, 1)
	c2 := make(chan string, 1)

	c1 <- "hello"
	c2 <- "world"

	select {
	case msg1 := <-c1:
		fmt.Println("c1 received: ", msg1)
	case msg2 := <-c2:
		fmt.Println("c2 received: ", msg2)
	default:
	}
}
```
两次执行的输出结果如下：
```
C:xxx\coder>go run main.go
c1 received:  hello

C:xxx\coder>go run main.go
c2 received:  world
```

### select 的超时
当 case 里的信道始终没有接收到数据，而且也没有 default 语句时，select 就会阻塞，但我们并不希望select 一直阻塞下去，这时我们可以手动设置一个超时时间

```
package main

import (
	"fmt"
	"time"
)

func main() {
	c1 := make(chan string, 1)
	c2 := make(chan string, 1)
	timeout := make(chan bool, 1)

	go makeTimeout(timeout, 2)

	select {
	case msg1 := <-c1:
		fmt.Println("c1 received: ", msg1)
	case msg2 := <-c2:
		fmt.Println("c2 received: ", msg2)
	case <-timeout:
		fmt.Println("Timeout, exit.")
	}
}

func makeTimeout(ch chan bool, t int) {
	time.Sleep(time.Second * time.Duration(t))
	ch <- true
}
```
代码执行输出结果如下：
```
Timeout, exit.
```

### select 对信道的读取和写入都可以
select 里的 case 表达式只要求是对信道的操作，不管时往信道写入数据还是读取数据，都可以

```
package main

import (
	"fmt"
)

func main() {
	c1 := make(chan int, 2)

	c1 <- 1

	select {
	case c1 <- 2:
		fmt.Println("c1 = ", <-c1)
		fmt.Println("c1 = ", <-c1)
	default:
		fmt.Println("channel blocking")
	}
}
```
输出结果如下：
```
c1 =  1
c1 =  2
```

### 总结
- select 只能用于 channel 操作，而 switch 更通用一些
- select 的 case 是随机执行的，而 switch 里的 case 是顺序执行的
- select 要注意避免死锁，也可以自己实现超时机制