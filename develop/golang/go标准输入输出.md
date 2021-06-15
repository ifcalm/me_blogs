## 标准库输入输出

- 标准输入
- 标准输出


### fmt.Scan应用案例--标准输出
```
package main
import "fmt"

func main() {

    var name string
    var age int

        //使用"&"获取score变量的内存地址(即取变量内存地址的运算符)，通过键盘输入为score变量指向的内存地址赋初值。

        //fmt.Scan是一个阻塞的函数，如果它获取不到数据就会一直阻塞哟。

        //fmt.Scan可以接收多个参数，用户输入参数默认使用空格或者回车换行符分割输入设备传入的参数，直到接收所有的参数为止

    fmt.Scan(&name, &age)
    fmt.Println(name, age)
}
```

### fmt.Scanln应用案例--标准输出

```
package main
import "fmt"

func main() {

    var name string
    var age int

        //和fmt.Scan功能类似，fmt.Scanln也是一个阻塞的函数，如果它获取不到数据就会一直阻塞哟。

        //fmt.Scanln也可以接收多个参数，用户输入参数默认使用空格分割输入设备传入的参数，遇到回车换行符就结束接收参数

    fmt.Scanln(&name, &age)
    fmt.Println(name, age)

}
```

### fmt.Scanf应用案例--标准输出

```
package main

import "fmt"

func main() {

    var name string
    var age int

    /*
        和fmt.Scanln功能类似，fmt.Scanf也是一个阻塞的函数，如果它获取不到数据就会一直阻塞哟。

        其实fmt.Scanln和fmt.Scanf可都以接收多个参数，用户输入参数默认使用空格分割输入设备传入的参数，遇到回车换行符就结束接收参数

        唯一区别就是可以格式化用户输入的数据类型,如下所示:
            %s:
                表示接收的参数会被转换成一个字符串类型，赋值给变量
            %d:
                表示接收的参数会被转换成一个整形类型，赋值给变量

        生产环境中使用fmt.Scanln和fmt.Scanf的情况相对较少，一般使用fmt.Scan的情况较多
    */

    fmt.Scanf("%s%d", &name, &age)
    fmt.Println(name, age)
}
```

`Scanln` 扫描来自标准输入的文本，将空格分隔的值依次存放到后续的参数内，直到碰到换行。`Scanf` 与其类似，除了 `Scanf` 的第一个参数用作格式字符串，用来决定如何读取。`Sscan` 和以 `Sscan` 开头的函数则是从字符串读取，除此之外，与 `Scanf` 相同。如果这些函数读取到的结果与您预想的不同，您可以检查成功读入数据的个数和返回的错误

**也可以使用 `bufio` 包提供的缓冲读取来读取数据**

```
package main
import (
    "fmt"
    "bufio"
    "os"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    fmt.Println("Please enter some input: ")
    input, err := inputReader.ReadString('\n')
    if err == nil {
        fmt.Printf("The input was: %s\n", input)
    }
}
```

`inputReader` 是一个指向 `bufio.Reader` 的指针。`inputReader := bufio.NewReader(os.Stdin)` 这行代码，将会创建一个读取器，并将其与标准输入绑定

`bufio.NewReader()` 构造函数的签名为：`func NewReader(rd io.Reader) *Reader`该函数的实参可以是满足 `io.Reader` 接口的任意对象，函数返回一个新的带缓冲的 `io.Reader` 对象，它将从指定读取器读取内容

返回的读取器对象提供一个方法 `ReadString(delim byte)`，该方法从输入中读取内容，直到碰到 `delim` 指定的字符，**然后将读取到的内容连同 `delim` 字符一起放到缓冲区**

`ReadString` 返回读取到的字符串，如果碰到错误则返回 `nil`。如果它一直读到文件结束，则返回读取到的字符串和 `io.EOF`。如果读取过程中没有碰到 `delim` 字符，将返回错误 `err != nil`

在上面的例子中，我们会读取键盘输入，直到回车键`\n`被按下, 屏幕是标准输出 `os.Stdout`, `os.Stderr` 用于显示错误信息，大多数情况下等同于 `os.Stdout`


### 循环读取输入

```
package main

import (
	"fmt"
	"io"
)

func main() {
	var a, b int
	for {
        //参数之间使用空格界别
		_, err := fmt.Scan(&a, &b)
		if err == io.EOF {
			break
		}
		fmt.Println(a + b)
	}
}
```

**另一种方式, 按行读取:**
```
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
    "strings"
)

func main() {
	inputReader := bufio.NewReader(os.Stdin)
	for {
		input, err := inputReader.ReadString('\n')
		if err == io.EOF {
			break
		} else if err != nil {
			break
		}
        //替换掉最后的换行符，以免造成格式报错
        input = strings.Replace(input, "\n", "", -1)
		s := []byte(input)
		start, end := 0, len(s)-1
		for start < end {
			s[start], s[end] = s[end], s[start]
			start++
			end--
		}
		fmt.Print(string(s))
	}
}
```

------------------------------------------------

### fmt.Println应用案例

```
package main

import "fmt"

func main() {

    name := "ifcalm"
    age := 18

    fmt.Println("======================")
    //将变量的值取出来打印并换行
    fmt.Println(name, age)
    fmt.Println("======================")
}
```

### fmt.Print应用案例

```
package main

import "fmt"

func main() {

    name := "ifcalm"
    age := 18

    fmt.Println("======================")
    //将变量的值取出来打印但不换行
    fmt.Print(name, age)
    fmt.Println("======================")
}
```

### fmt.Printf应用案例

```
package main

import "fmt"

func main() {

    name := "ifcalm"
    age := 18

    fmt.Println("======================")

    /*
        常用的占位符:
            %s:
                表示一个字符串类型
            %d:
                表示一个整形类型
            %T:
                表示一个变量对应的数据类型
            %f:
                表示一个浮点数类型
            %t:
                表示一个布尔类型
            %c:
                表示一个字符类型
            %p:
                表示一个指针类型
            \t:
                表示一个制表符
            \n:
                表示换行符

        可以使用占位符格式化打印结果，如下所示。
    */
    fmt.Printf("姓名:%s\t年龄:%d\n", name, age)

    fmt.Println("======================")
}
```