## Golang 几个简单应用

### reflection(反射)
#### 反射获取基本类型
可以在运行时动态获取变量的相关信息
  
* reflect.TypeOf() -->获取变量类型
* reflect.ValueOf() -->获取变量的值
* reflect.Value.Kind() -->获取变量的类别，返回一个常数
* reflect.Value.Interface() -->转换成 interface{}类型

```
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x := 3.4
	fmt.Println("type:", reflect.TypeOf(x))
	v := reflect.ValueOf(x)
	fmt.Println("value:", v)

	fmt.Println("type:", v.Type())
	fmt.Println("kind:", v.Kind())
	fmt.Println("value:", v.Float())

	fmt.Println(v.Interface())
}
```

#### 反射获取结构体
```
package main

import (
	"fmt"
	"reflect"
)

type Student struct {
	Name  string
	Age   int
	Score float32
}

func test(b interface{}) {
	t := reflect.TypeOf(b)
	fmt.Println(t)
}

func main() {
	var a Student = Student{
		Name:  "stu",
		Age:   18,
		Score: 92,
	}
	test(a)
}
```

#### Elem 反射

#### 反射调用结构体

#### Elem反射获取tag
```
type Student struct {
    Name string `json: "stu_name"`
    Age int
    Score float32
}
```

### json 协议
#### 结构体转json
```
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	UserName string `json:"username"`
	NickName string `json:"nickname"`
	Age      int
	Birthday string
	Sex      string
	Email    string
	Phone    string
}

func testStruct() {
	user1 := &User{
		UserName: "usere1",
		NickName: "Murphy",
		Age:      18,
		Birthday: "1995/09/16",
		Sex:      "男",
		Email:    "123456@qq.com",
		Phone:    "15600000000",
	}

	data, err := json.Marshal(user1)
	if err != nil {
		fmt.Println("json.marshal failed, err:", err)
		return
	}
	fmt.Printf("%s\n", string(data))
}

func main() {
	testStruct()
	fmt.Println("-----")
}
```

#### map转json

#### int 转 json

#### slice 转 json

#### json 反序列化为结构体

#### json 反序列化为 map


### 终端读取
读取用户的输入，最简单的办法是使用 fmt 包提供的 `Scan` 和 `Sscan` 开头的函数

```
package main

import "fmt"

var (
	firstName, lastName, s string
	i                      int
	f                      float32
	input                  = "56.12 / 5212 / Go"
	format                 = "%f / %d / %s"
)

func main() {
	fmt.Println("Please enter your full name: ")
	fmt.Scanln(&firstName, &lastName)
	fmt.Scanf("%s %s", &firstName, &lastName)
	fmt.Printf("Hi %s %s!\n", firstName, lastName)
	fmt.Sscanf(input, format, &f, &i, &s)
	fmt.Println("From the string we read: ", f, i, s)
}
```

#### 命令行参数 os.Args
```
package main

import (
	"fmt"
	"os"
)

func main() {
	//os.Args 提供原始命令行参数访问功能。注意，切片中的第一个参数是该程序的路径，并且 os.Args[1:]保存所有程序的的参数。
	argsWithProg := os.Args
	argsWithoutProg := os.Args[1:]
	//你可以使用标准的索引位置方式取得单个参数的值。
	arg := os.Args[3]
	fmt.Println(argsWithProg)
	fmt.Println(argsWithoutProg)
	fmt.Println(arg)
}
```

#### 命令行参数 falg
```
package main

import (
	"flag"
	"fmt"
)

func main() {
	/*
		基本的标记声明仅支持字符串、整数和布尔值选项。
		这里我们声明一个默认值为 "foo" 的字符串标志 "word"
		flag.String 函数返回一个字符串指针（不是一个字符串值)
	*/
	wordPtr := flag.String("word", "foo", "a string")
	//使用和声明 word 标志相同的方法来声明 numb 和 fork 标志。
	numbPtr := flag.Int("numb", 42, "an int")
	boolPtr := flag.Bool("fork", false, "a bool")
	//用程序中已有的参数来声明一个标志也是可以的。注意在标志声明函数中需要使用该参数的指针。
	var svar string
	flag.StringVar(&svar, "svar", "bar", "a string var")
	//所有标志都声明完成以后，调用 flag.Parse() 来执行命令行解析。
	flag.Parse()

	fmt.Println("word:", *wordPtr)
	fmt.Println("numb:", *numbPtr)
	fmt.Println("fork:", *boolPtr)
	fmt.Println("svar:", svar)
	fmt.Println("tail:", flag.Args())
}
```

### 文件操作
os包

#### 文件创建

#### 文件写入

#### 文件读取

#### 文件删除

#### 压缩文件读写
`import "archive/tar"`
tar包实现了tar格式压缩文件的存取。本包目标是覆盖大多数tar的变种，包括GNU和BSD生成的tar文件。

#### 写入内容到 Excel
`import "encoding/csv"`

csv读写逗号分隔值（csv）的文件。

一个csv分拣包含零到多条记录，每条记录一到多个字段。每个记录用换行符分隔。最后一条记录后面可以有换行符，也可以没有

#### 日志文件
`import "log"`

log包实现了简单的日志服务。本包定义了Logger类型，该类型提供了一些格式化输出的方法。本包也提供了一个预定义的“标准”Logger，可以通过辅助函数`Print[f|ln]`、`Fatal[f|ln]`和`Panic[f|ln]`访问，比手工创建一个Logger对象更容易使用。
Logger会打印每条日志信息的日期、时间，默认输出到标准错误。
Fatal系列函数会在写入日志信息后调用`os.Exit(1)`。
Panic系列函数会在写入日志信息后panic

### server 服务

#### 服务端
监听端口，接收客户端的链接，创建goroutine,处理该链接
```
package main

import (
    "fmt"
    "net"
)

func main() {
    fmt.Println("start server...")
    listen, err := net.Listen("tcp", "0.0.0.0:50000")
    if err != nil {
        fmt.Println("listen failed, err:", err)
        return
    }
    for {
        conn, err := listen.Accept()
        if err != nil {
            fmt.Println("accept failed, err:", err)
            continue
        }
        go process(conn)
    }
}
func process(conn net.Conn) {
    defer conn.Close()
    for {
        buf := make([]byte, 512)
        n, err := conn.Read(buf)
        if err != nil {
            fmt.Println("read err:", err)
            return
        }

        fmt.Printf(string(buf[0:n]))
    }
}
```

#### 客户端
a. 建立与服务端的链接
b. 进行数据收发
c. 关闭链接

```
package main

import (
    "bufio"
    "fmt"
    "net"
    "os"
    "strings"
)

func main() {

    conn, err := net.Dial("tcp", "localhost:50000")
    if err != nil {
        fmt.Println("Error dialing", err.Error())
        return
    }

    defer conn.Close()
    inputReader := bufio.NewReader(os.Stdin)
    for {
        input, _ := inputReader.ReadString('\n')
        trimmedInput := strings.Trim(input, "\r\n")
        if trimmedInput == "Q" {
            return
        }
        _, err = conn.Write([]byte(trimmedInput))
        if err != nil {
            return
        }
    }
}
```

### 时间处理
#### 格式化

```
package main

import (
	"fmt"
	"time"
)

func main() {

	// 当前时间戳
	now := time.Now().Unix()
	fmt.Println(now)

	// 当前格式化时间
	fmt.Println(time.Now().Format("2006-01-02 15:04:05"))
	// 这是个奇葩,必须是这个时间点, 据说是go诞生之日, 记忆方法:6-1-2-3-4-5

	// 时间戳转str格式化时间
	str_time1 := time.Unix(0, 0).Format("2006-01-02 15:04:05")
	fmt.Println(str_time1)
	str_time2 := time.Unix(1522393808, 0).Format("2006年01月02日 15时04分05秒")
	fmt.Println(str_time2)
	// str_time3 := time.Unix("1522393808",0).Format("2006-01-02 15:04:05")
	// cannot use "1522393808" (type string) as type int64 in argument to time.Unix
	str_time4 := time.Unix(1522393808, 0).Format("06-01-02 15:04:05")
	fmt.Println(str_time4)
	str_time5 := time.Unix(1522393808, 0).Format("01-02 15:04")
	fmt.Println(str_time5)

	// str格式化时间转时间戳
	// 方法一   2018-03-30 15:24:59
	the_time := time.Date(2018, 3, 30, 15, 24, 59, 0, time.Local)
	unix_time := the_time.Unix()
	fmt.Println(unix_time)
	fmt.Println(time.Unix(unix_time, 0).Format("2006-01-02 15:04:05"))
	// 方法二 , 使用time.Parse
	/*
	   返回的不是本地时间, 而是 UTC , 会自动加8小时.
	*/
	the_time, err := time.Parse("2006-01-02 15:04:05", "2018-03-30 15:24:59")
	if err == nil {
		unix_time := the_time.Unix()
		fmt.Println(unix_time)
		fmt.Println(time.Unix(unix_time, 0).Format("2006-01-02 15:04:05"))
	}
	// 使用time.ParseInLocation
	the_time, err = time.ParseInLocation("2006-01-02 15:04:05", "2018-03-30 15:24:59", time.Local)
	if err == nil {
		unix_time := the_time.Unix()
		fmt.Println(unix_time)
		fmt.Println(time.Unix(unix_time, 0).Format("2006-01-02 15:04:05"))
	}

	// 格式化当前时间
	lasttime := time.Now().Format("2006-01-02 15:04:05")
	fmt.Println(lasttime)

}
```

### 锁机制

#### 互斥锁
golang中sync包实现了两种锁Mutex （互斥锁）和RWMutex（读写锁），其中RWMutex是基于Mutex实现的，只读锁的实现使用类似引用计数器的功能。

1、互斥锁 （用于基本上全是写入操作的应用）
```
type Mutex
	func (m *Mutex) Lock()
	func (m *Mutex) Unlock()
```

go mutex是互斥锁，只有Lock和Unlock两个方法，在这两个方法之间的代码不能被多个goroutins同时调用到。

其中Mutex为互斥锁，Lock()加锁，Unlock()解锁，使用Lock()加锁后，便不能再次对其进行加锁，直到利用Unlock()解锁对其解锁后，才能再次加锁．适用于读写不确定场景，即读写次数没有明显的区别，并且只允许只有一个读或者写的场景，所以该锁叶叫做全局锁。

func (m *Mutex) Unlock()用于解锁m，如果在使用Unlock()前未加锁，就会引起一个运行错误．已经锁定的Mutex并不与特定的goroutine相关联，这样可以利用一个goroutine对其加锁，再利用其他goroutine对其解锁。

互斥锁只能锁定一次，当在解锁之前再次进行加锁，便会死锁状态，如果在加锁前解锁，便会报错“panic: sync: unlock of unlocked mutex”

#### 读写锁
用于读取多，写入少的操作应用

```
type RWMutex

    func (rw *RWMutex) Lock()

    func (rw *RWMutex) RLock()

    func (rw *RWMutex) RLocker() Locker

    func (rw *RWMutex) RUnlock()

    func (rw *RWMutex) Unlock()
```

RWMutex是一个读写锁，该锁可以加多个读锁或者一个写锁，其经常用于读次数远远多于写次数的场景．

func (rw *RWMutex) Lock()　　写锁，如果在添加写锁之前已经有其他的读锁和写锁，则lock就会阻塞直到该锁可用，为确保该锁最终可用，已阻塞的 Lock 调用会从获得的锁中排除新的读取器，即写锁权限高于读锁，有写锁时优先进行写锁定
func (rw *RWMutex) Unlock()　写锁解锁，如果没有进行写锁定，则就会引起一个运行时错误

func (rw *RWMutex) RLock() 读锁，当有写锁时，无法加载读锁，当只有读锁或者没有锁时，可以加载读锁，读锁可以加载多个，所以适用于＂读多写少＂的场景

func (rw *RWMutex)RUnlock()　读锁解锁，RUnlock 撤销单次RLock 调用，它对于其它同时存在的读取器则没有效果。若 rw 并没有为读取而锁定，调用 RUnlock 就会引发一个运行时错误(注：这种说法在go1.3版本中是不对的，例如下面这个例子)。

读写锁的写锁只能锁定一次，解锁前不能多次锁定，读锁可以多次，但读解锁次数最多只能比读锁次数多一次，一般情况下我们不建议读解锁次数多余读锁次数。

读多写少的情况，用读写锁， 携程同时在操作读。

### 原子操作
sync/atomic 库提供了原子操作的支持，原子操作直接有底层CPU硬件支持，因而一般要比基于操作系统API的锁方式效率高些。本文对 sync/atomic 中的基本操作进行一个简单的介绍。

原子增、减值
用于对变量值进行原子增操作，并返回增加后的值。

第一个参数值必须是一个指针类型的值，以便施加特殊的CPU指令。
第二个参数值的类型和第一个被操作值的类型总是相同的

```
package main 

import(
    "fmt"
    "sync"
    "sync/atomic"
)

func main(){
    var sum uint32 = 100
    var wg sync.WaitGroup
    for i := 0; i < 50; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            atomic.AddUint32(&sum, 1)
        }()
    }
    wg.Wait()
    fmt.Println(sum)
}
```

### 加密解密

#### md5
```
import (
	"crypto/md5"
)
```

#### base64
golang中base64的编码和解码可以用内置库`encoding/base64`

#### sha

#### hmac
HMAC是密钥相关的哈希运算消息认证码，HMAC运算利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出。

主要用于验证接口签名

### 常用算法
#### 冒泡排序
```
package main

import (
	"fmt"
)

var sli = []int{1, 43, 54, 62, 21, 66, 32, 78, 36, 76, 39}

func bubbleSort(sli []int) []int {
	len := len(sli)
	//该层循环控制 需要冒泡的轮数
	for i := 1; i < len; i++ {
		//该层循环用来控制每轮 冒出一个数 需要比较的次数
		for j := 0; j < len-1; j++ {
			if sli[i] < sli[j] {
				sli[i], sli[j] = sli[j], sli[i]
			}
		}
	}
	return sli
}

func main() {
	res := bubbleSort(sli)
	fmt.Println(res)
}
```
#### 选择排序
```
package main

import (
	"fmt"
)

var sli = []int{1, 43, 54, 62, 21, 66, 32, 78, 36, 76, 39}

func selectSort(sli []int) []int {
	//双重循环完成，外层控制轮数，内层控制比较次数
	len := len(sli)
	for i := 0; i < len-1; i++ {
		//先假设最小的值的位置
		k := i
		for j := i + 1; j < len; j++ {
			//sli[k] 是当前已知的最小值
			if sli[k] > sli[j] {
				//比较，发现更小的,记录下最小值的位置；并且在下次比较时采用已知的最小值进行比较。
				k = j
			}
		}
		//已经确定了当前的最小值的位置，保存到 k 中。如果发现最小值的位置与当前假设的位置 i 不同，则位置互换即可。
		if k != i {
			sli[k], sli[i] = sli[i], sli[k]
		}
	}
	return sli
}

func main() {
	res := selectSort(sli)
	fmt.Println(res)
}
```
#### 快速排序
```
package main

import (
	"fmt"
)

var sli = []int{1, 43, 54, 62, 21, 66, 32, 78, 36, 76, 39}

func quickSort(sli []int) []int {
	//先判断是否需要继续进行
	len := len(sli)
	if len <= 1 {
		return sli
	}
	//选择第一个元素作为基准
	base_num := sli[0]
	//遍历除了标尺外的所有元素，按照大小关系放入左右两个切片内
	//初始化左右两个切片
	left_sli := []int{}  //小于基准的
	right_sli := []int{} //大于基准的
	for i := 1; i < len; i++ {
		if base_num > sli[i] {
			//放入左边切片
			left_sli = append(left_sli, sli[i])
		} else {
			//放入右边切片
			right_sli = append(right_sli, sli[i])
		}
	}

	//再分别对左边和右边的切片进行相同的排序处理方式递归调用这个函数
	left_sli = quickSort(left_sli)
	right_sli = quickSort(right_sli)

	//合并
	left_sli = append(left_sli, base_num)
	return append(left_sli, right_sli...)
}
func main() {
	res := quickSort(sli)
	fmt.Println(res)
}
```
#### 插入排序
```
package main

import (
	"fmt"
)

var sli = []int{1, 43, 54, 62, 21, 66, 32, 78, 36, 76, 39}

func insertSort(sli []int) []int {
	len := len(sli)
	for i := 0; i < len; i++ {
		tmp := sli[i]
		//内层循环控制，比较并插入
		for j := i - 1; j >= 0; j-- {
			if tmp < sli[j] {
				//发现插入的元素要小，交换位置，将后边的元素与前面的元素互换
				sli[j+1], sli[j] = sli[j], tmp
			} else {
				//如果碰到不需要移动的元素，则前面的就不需要再次比较了。
				break
			}
		}
	}
	return sli
}

func main() {
	res := insertSort(sli)
	fmt.Println(res)
}
```

#### 睡眠排序
```
package main

import (
	"fmt"
	"time"
)

func main() {
	tab := []int{1, 3, 0, 5}

	ch := make(chan int)
	for _, value := range tab {
		go func(val int) {
			time.Sleep(time.Duration(val) * 10000000)
			fmt.Println(val)
			ch <- val
		}(value)
	}

	for _ = range tab {
		<-ch
	}
}
```

### 限流器

### 日志包

### 随机数

### 编码格式转换


