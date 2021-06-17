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


### 切片的初始化

```
s := []string{}   //创建一个空切片
s_2 := make([]string, 5)   //使用 make 创建切片
var s_3 []string
```

### 字典的初始化

```
var m map[string]string

m_1 := map[string]string{
    "name": "lss",
    "age": "25",
}
```

### golang 计算四舍五入

```
package main
 
import (
    "fmt"
    "math"
)
 
func main() {
    var s float64
    for {
        _, err := fmt.Scanln(&s)
        if err != nil {
            break
        }
        fmt.Println(int(math.Floor(s+0.5)))
    }
}
```

go官方的math 包中提供了取整的方法:
- `math.Ceil()`, 向上取整
- `math.Floor()`, 向下取整

golang没有类似python的round()函数，可以让原数字先+0.5，然后向下取整


----------------------------------------------

### 初始化切片的两种方式

- make([]int, 5)
- make([]int, 0)

初始化切片的时候，指定大于0的长度时，切片会初始化指定长度的数据，例如`make([]int, 5) => [0 0 0 0 0]`, 而`make([]int, 0) => []`

所以在 使用 `append()` 的时候，指定长度的切片会从长度以后的位置开始添加，即 `[0 0 0 0 0 2]`, 而长度为0的切片为 `[2]`

所以在使用切片的时候要注意`初始化时指定长度的默认值`对使用的影响

-------------------------------------------------------

### 链表定义

```
type ListNode struct {
    Data interface{}
    Next *ListNode
}

func NewListNode() *ListNode {
    return &ListNode{}
}
```

### 二叉树定义

```
type TreeNode struct {
    Data interface{}
    LeftChild *TreeNode
    RightChild *TreeNode
}

func NewTreNode() *TreeNode {
    return &TreeNode{}
}
```