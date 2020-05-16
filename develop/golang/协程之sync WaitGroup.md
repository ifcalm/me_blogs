## sync.WaitGroup

为了保证main goroutine 在所有的 goroutine 都执行完成后在退出，有以下几种方法：
- time.Sleep （不推荐）
- 使用信道来标记
- 使用 WaitGroup

### 使用信道来标记完成
不要通过共享内存来通信，要通过通信来共享内存

信道可以实现多个协程间的通信，我们只要定义一个信道，在任务完成后，往信道中写入 true，然后主协程获取到true，就认为协程已经执行完毕
```
package main

import "fmt"

func main() {
	done := make(chan bool)
	go func() {
		for i := 0; i < 5; i++ {
			fmt.Println(i)
		}
		done <- true
	}()
	<-done
}
```
输出结果为:
```
0
1
2
3
4
```

### 使用 WaitGroup

waitGroup 由 sync 包提供，只要实例化就能使用
```
var x sync.WaitGroup
```

WaitGroup的几个方法：
- `Add` 初始值为0，传入的值会往计数器上加，这里直接传子协程的数量
- `Done` 当某个子协程完成后，可调用此方法，会从计数器上减一，通常可以使用 defer 来调用
- `Wait` 阻塞当前协程，知道实例里的计数器归零

```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup

	wg.Add(2)
	go testA(1, &wg)
	go testA(2, &wg)
	wg.Wait()
}

func testA(x int, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 0; i < 5; i++ {
		fmt.Printf("工作者 %d: %d\n", x, i)
	}
}
```
上面代码输出结果为:
```
工作者 1: 0
工作者 1: 1
工作者 1: 2
工作者 1: 3
工作者 1: 4
工作者 2: 0
工作者 2: 1
工作者 2: 2
工作者 2: 3
工作者 2: 4
```
