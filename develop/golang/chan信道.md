## 信道/通道 chan

### 信道的定义
每个信道只能传递一种数据类型，因此在声明的时候，需要指定数据类型。
```
var x chan int
```
声明后的信道，零值是`nil`，无法直接使用，需配合`make`函数进行初始化
```
x = make(xhan int)
```

上面表示方法也可以合并成一句：
```
x := make(chan int)
```

### 信道的操作
信道操作分为：往信道中发送数据 和 从信道中读取数据

```
x := make(chan int)
x <- 3              //往信道中发送数据

y := <- x           //从信道中读取数据，赋值给y
```

信道用完，可以对其进行关闭，避免等待
```
close(x)
```

对已关闭的信道在关闭，是会报错的，下面是判断一个信道是否被关闭：
```
m, ok := <- x
```

### 信道的容量与长度
当信道的容量为 0 时，说明信道中不能存放数据，在发送数据时，必须有人立即接收，这种称为 无缓冲信道

当信道容量 >= 1 时，信道可以存放多个数据。

信道可以理解为一个容器。

信道的容量可以使用 `cap` 函数获取，信道的长度可以使用 `len` 函数来获取。

```
package main

import "fmt"

func main() {
	x := make(chan int, 10)
	fmt.Printf("信道可缓存 %d 个数据\n", cap(x))
	x <- 2
	x <- 5
	fmt.Println("信道中当前有 %d 个数据", len(x))
}
```
输出结果为:
```
信道可缓存 10 个数据
信道中当前有 %d 个数据 2
```

### 双向信道与单向信道
一般情况下，我们定义的信道都是双向信道，可以发送数据，也可以接收数据。

但有时候，我们需要对信道的数据流向做一些控制，比如某个信道只能发送数据或者只能接收数据。

默认情况下，信道是双向的:
```
package main

import (
	"fmt"
	"time"
)

func main() {
	x := make(chan int)
	go func() {
		fmt.Println("准备发送数据：1")
		x <- 1
	}()

	go func() {
		num := <-x
		fmt.Printf("接收到的数据是：%d", num)
	}()

	time.Sleep(1)
}
```
输出结果为：
```
准备发送数据：1
接收到的数据是：1
```

定义只读信道：
```
x := make(<-chan int)
```

定义只写信道：
```
x := make(chan<- int)
```

### 信道的遍历
信道遍历可以使用 `for + range`，在遍历时要确保信道是处于关闭状态，不然循环会阻塞。
```
package main

import "fmt"

func main() {
	a := make(chan int, 10)
	go testA(a)
	for k := range a {
		fmt.Println(k)
	}
}

func testA(c chan int) {
	n := cap(c)
	x, y := 1, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}
```

### 信道来做锁
当信道里的数据量达到设定的容量时，在往里发送数据会阻塞程序，利用这个特性，可以来当程序的锁。
```
package main

import (
	"fmt"
	"time"
)

func main() {
	a := make(chan bool, 1) //使用容量为1的信道可以达到锁的效果
	var x int
	for i := 0; i < 1000; i++ {
		go testA(a, &x)
	}

	time.Sleep(3)
	fmt.Println("x 的值：", x)
}

func testA(ch chan bool, x *int) {
	ch <- true
	*x = *x + 1
	<-ch
}
```
上面程序输出结果为 1000


不加锁的代码如下：
```
package main

import (
	"fmt"
	"time"
)

func main() {
	var x int
	for i := 0; i < 1000; i++ {
		go testA(&x)
	}

	time.Sleep(3)
	fmt.Println("x 的值：", x)
}

func testA(x *int) {
	*x = *x + 1
}
```
输出结果小于 1000

### 信道死锁案例
