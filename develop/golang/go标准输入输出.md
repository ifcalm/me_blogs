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