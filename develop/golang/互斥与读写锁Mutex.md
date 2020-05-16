## 互斥锁与读写锁

面对并发问题，优先考虑使用信道，如果通过信道解决不了，不得不使用共享内存来实现并发编程的，那么Go中的锁机制，就是需要我们掌握的了！

sync包下有两个很重要的锁类型：
- `Mutex` 使用它可以实现互斥锁
- `RWMutex` 使用它可以实现读写锁

### 互斥锁 Mutex
使用互斥锁是为了保护一个资源不会因为并发操作而引起冲突导致数据不准确

```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	count := 0
	wg.Add(3)
	go testA(&count, &wg)
	go testA(&count, &wg)
	go testA(&count, &wg)
	wg.Wait()
	fmt.Print("count的值为：", count)
}

func testA(count *int, wg *sync.WaitGroup) {
	for i := 0; i < 100000; i++ {
		*count = *count + 1
	}
	wg.Done()
}
```
如上代码的执行结果是小于等于 300000 的一个值，理论上因该是300000，原因就在于这3个协程在执行时，先读取count的值，在更新count的值，而这个过程不具备原子性，所以导致了数据不准确，解决这个问题，就是给 testA 这个函数加上一个 Mutex 互斥锁，要求同一时刻，仅能有一个协程对count 操作。

### 锁的定义
```
var lock *sync.Mutex
lock = new(sync.Mutex)
```
```
lock := &sync.Mutext{}
```

通过加锁，不管执行多少次，输出都只有一个结果：
```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	lock := &sync.Mutex{}
	count := 0
	wg.Add(3)
	go testA(&count, &wg, lock)
	go testA(&count, &wg, lock)
	go testA(&count, &wg, lock)
	wg.Wait()
	fmt.Print("count的值为：", count)
}

func testA(count *int, wg *sync.WaitGroup, lock *sync.Mutex) {
	for i := 0; i < 100000; i++ {
		lock.Lock()
		*count = *count + 1
		lock.Unlock()
	}
	wg.Done()
}
```

注意：加了锁之后，别忘了解锁，必要时使用 defer 语句。


### 读写锁 RWMutex
- 为了保证数据的安全，它规定当有人还在读取数据时，不允许有人更新这个数据，即读锁占用，写锁会阻塞
- 为了保证程序的效率，多人读取数据时，互不影响不会造成阻塞，不会像 Mutex 只允许一个人读取同一个数据

#### 定义 RWMutex 锁
```
var lock *sync.RWMutex
lock = new(sync.RWMutex)
```

```
lock := &sync.RWMutex{}
```

RWMutex 提供两种锁，每种锁对于两个方法，为了避免死锁，两个方法应同时出现，必要时请使用 defer

- 读锁：使用 `RLock` 方法开启锁，调用 `Runlock` 释放锁
- 写锁：使用 `Lock` 方法开启锁，使用 `Unlock` 释放锁


```
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	lock := sync.RWMutex{}
	lock.Lock()

	for i := 0; i < 4; i++ {
		go func(i int) {
			fmt.Printf("第 %d 个协程准备开始...\n", i)

			lock.RLock()
			fmt.Printf("第 %d 个协程获得读锁, sleep 1s 后, 释放锁\n", i)
			time.Sleep(time.Second)
			lock.RUnlock()
		}(i)
	}

	time.Sleep(time.Second * 2)

	fmt.Println("准备释放写锁，读锁不在阻塞")
	lock.Unlock()
    
    //等到读锁全部释放，才能获得写锁
    //上面4个协程全部完成才能往下走
	lock.Lock()
	fmt.Println("程序退出...")
	lock.Unlock()
}
```

上面程序的输出结果为:
```
第 0 个协程准备开始...
第 1 个协程准备开始...
第 3 个协程准备开始...
第 2 个协程准备开始...
准备释放写锁，读锁不在阻塞
第 2 个协程获得读锁, sleep 1s 后, 释放锁
第 1 个协程获得读锁, sleep 1s 后, 释放锁
第 0 个协程获得读锁, sleep 1s 后, 释放锁
第 3 个协程获得读锁, sleep 1s 后, 释放锁
程序退出...
```

程序解释：写锁占用的时候，读锁阻塞，写锁释放后，读锁就自由了，然后读锁全部释放之后，才能获得写锁。

所以输出结果需要写锁执行完成，然后读锁在执行完成，最后在去执行写锁。即4个协程执行完成才能往下走。