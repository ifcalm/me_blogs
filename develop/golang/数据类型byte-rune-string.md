## byte rune string

### byte与rune
`byte` 占用一个字节，即8个比特位，和 `uint8`类型本质上没什么区别，它表示的是 `ACSII` 表中的一个字符

```
package main

import (
	"fmt"
)

func main() {
	var a byte = 65
	var b uint8 = 66
	fmt.Printf("a的值: %c \n b的值: %c", a, b)
}
```
输出结果为:
```
a的值: A
 b的值: B
```

`rune` 占用 4 个字节，即 32 个比特位，和`uint32`类型本质上没什么区别，它表示的是一个 `Unicode` 字符，`Unicode`是一个可以表示世界范围内绝大部分字符的编码规范
```
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var a byte = 'A'
	var b rune = 'B'
	fmt.Printf("a 占用 %d 个字节数\n b 占用 %d 个字节数", unsafe.Sizeof(a), unsafe.Sizeof(b))
}
```
输出结果如下：
```
a 占用 1 个字节数
 b 占用 4 个字节数
```

`byte` 类型能表示的值有限，只有2^8=256 个，所以如果想表示中文字符的话，只能使用 `rune` 类型

在使用 `byte`和`rune`定义字符时，我们都使用单引号，而没有使用双引号，在Go中单引号与双引号并不是等价的

单引号用来表示字符，双引号用来表示字符串


### 字符串
定义一个字符串变量
```
var mystr string = "hello"
```

字符分为 `byte` 与 `rune` 类型，只是占用大小不同

Go语言的`string` 是用 `utf-8` 进行编码的，英文字母占用一个字节，中文字占用3个字节。
例如"hello,中国" 占用 `5+1+(3*2)=12` 个字节