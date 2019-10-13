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
