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
```

```