## 协程（goroutine）
一个 goroutine 本身就是一个函数，当你直接调用时，它就是一个普通函数，如果你在调用前加一个关键字 `go`，那就开了一个 goroutine

```
func()      //执行一个函数
go func()   //开启一个协程执行这个函数
```
