### Go 初始化二维切片

```
package main

import "fmt"

func main() {
	s := make([][]string, 5)
	for i := 0; i < 5; i++ {
		s[i] = make([]string, 5)  //把内层切片也进行初始化
	}
	fmt.Println(s)
}
```

**初始化二维切片的时候，也需要使用循环把内层的切片进行初始化**

**数组初始化的时候必须使用数字，不能够用变量代替**

