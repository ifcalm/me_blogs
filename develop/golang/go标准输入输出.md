## 标准库输入输出

- 标准输入
- 标准输出


### `io`-- 基本的 IO 接口

io 包为 I/O 原语提供了基本的接口, 在 io 包中最重要的是两个接口：`Reader` 和 `Writer` 接口

#### Reader 接口

Reader 接口的定义如下:

```
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

#### Writer 接口

Writer 接口的定义如下：

```
type Writer interface {
    Write(p []byte) (n int, err error)
}
```

### `ioutil`, 方便的IO操作函数集

待补充

### `fmt`, 格式化IO

待补充

### `bufio`, 缓存IO

待补充

