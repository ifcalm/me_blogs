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

### 函数的闭包

Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被“绑定”在了这些变量上。 例如，函数 adder 返回一个闭包。每个闭包都被绑定在其各自的 sum 变量上

```
package main
import "fmt"

func adder() func(int) int {
     sum := 0
     return func(x int) int {
         sum += x
         return sum
     }
}

func main() {
     pos, neg := adder(), adder()
     for i := 0; i < 10; i++ {
         fmt.Println(
             pos(i),
             neg(-2*i),
         )
     }
}
```

### 判断 key 是否在 map 中

```
if _, ok := m[key], ok {
    //存在
}

```

通过双赋值检测某个键是否存在： `elem, ok = m[key]` 若 key 在 m 中，ok 为 `true` ；否则，ok 为 `false`。 若 key 不在映射中，那么 elem 是该映射元素类型的零值。 同样的，当从映射中读取某个不存在的键时，结果是映射的元素类型的零值。 注 ：若 elem 或 ok 还未声明，你可以使用短变量声明： `elem, ok := m[key]`


