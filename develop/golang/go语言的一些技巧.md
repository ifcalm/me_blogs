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


### 函数作为值

函数也是值。它们可以像其它值一样传递。 函数值可以用作函数的参数或返回值

```
package main
import (
     "fmt"
     "math"
)

func compute(fn func(float64, float64) float64) float64 {
     return fn(3, 4)
}

func main() {
     hypot := func(x, y float64) float64 {
         return math.Sqrt(x*x + y*y)
     }
     fmt.Println(hypot(5, 12))

     fmt.Println(compute(hypot))
     fmt.Println(compute(math.Pow))
}
```

#### LeetCoode 17

```
func letterCombinations(digits string) []string {
    if len(digits) == 0 {
        return []string{}
    }
    mp := map[string]string {
        "2": "abc",
        "3": "def",
        "4": "ghi",
        "5": "jkl",
        "6": "mno",
        "7": "pqrs",
        "8": "tuv",
        "9": "wxyz",
    }

    con := []string{}
    var hs func(int, string)

    hs = func(i int, path string) {
        if i >= len(digits) {
            con = append(con, path)
            return
        }

        for _, v := range mp[string(digits[i])] {
            hs(i+1, path + string(v))
        }
    }
    hs(0, "")
    return con
}
```

