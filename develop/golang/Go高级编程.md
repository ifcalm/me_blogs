```
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

func main() {
	fmt.Println("Please visit http://127.0.0.1:12345/")
	http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
		s := fmt.Sprintf("你好，世界！, Time: %s", time.Now().String())
		fmt.Fprintf(w, "%v \n", s)
		log.Printf("%v \n", s)
	})

	if err := http.ListenAndServe(":12345", nil); err != nil {
		log.Fatal("ListenAndServe", err)
	}
}
```

## 数组-字符串-切片
在主流的编程语言中数组及其相关的数据结构是使用得最为频繁的，只有在它不能满足时才会考虑链表、散列表和更复杂的自定义数据结构

散列表可以看作是数组和链表的混合体

