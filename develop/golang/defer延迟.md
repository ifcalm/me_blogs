## defer 延迟调用
`defer` 是 `Go` 语言提供的一种用于注册延迟调用的机制，每一次 defer 都会把函数压入栈中。

`defer` 语句并不会马上执行，而是会进入一个栈，函数 `return` 前，会按照先进后出的顺序执行。

```
package main

import "fmt"

func main() {
	a := deferA()
	b := deferB()
    c := deferC()
	fmt.Println(a)
	fmt.Println(b)
    fmt.Println(c)
}

func deferA() int {
	var i int
	defer func() {
		i++
	}()
	return i
}

func deferB() (r int) {
	defer func() {
		r++
	}()
	return r
}

func deferC() (r int) {
	r = 1
	defer func(r int) {
		r = r + 5
	}(r)
	return
}
```
上面代码最终结果输出为 `0 1 1`

`defer` 语句定义时，对外部变量的引用分为`函数参数`和`闭包引用`两种，作为函数参数，在 `defer` 定义时就把值传给了 `defer`，并被缓存起来，对于闭包引用，则会在defer 函数真正调用时根据整个上下文确定当前的值。

`return xxx`
经过编译后，变成三条指令;
```
返回值 = xxx
调用 defer 函数 （这里可能会操作返回值）
空的 return
```

代码参考:
```
func test1() (r int) {
    r = 0               //赋值
    defer func() {      //闭包引用，返回值被修改
        r++
    }()
    return              //空的return
}


func test2() (r int) {
    r = 1                 //赋值
    defer func(r int) {   //做为函数参数，不会修改要返回的那个r
        r = r + 5
    }(r)
    return
}
在test2中，r 是作为函数参数使用，是一份赋值，defer 语句里面的r 和外面的 r其实是两个变量，不会互相影响。
```

### defer 应用场景
基本成对的操作都可以使用延迟函数，比如打开与关闭文件，申请与释放资源，或者前后存在依赖关系时，可以使用 `defer` 关键词来简化处理流程。

文件的打开与关闭:
```
func readFile(filename string) ([]byte, error) {
    f, err := os.Open(filename)
    if err != nil {
        retrun nil, err
    }
    defer f.Close()
    return ioutil.ReadAll(f)
}
```

锁的申请和释放是保证同步的一种重要机制：
```
var mu sync.Mutex
var m = make(map[string]int)
func locktest(key string) int {
    um.lock()
    defer mu.Unlock()
    return m[key]
}
```

### defer 的性能问题


### defer 的坑