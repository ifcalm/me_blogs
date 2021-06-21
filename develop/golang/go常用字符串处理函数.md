## 字符串处理

## strings 包

由于 string 类型可以看成是一种特殊的 slice 类型，因此获取长度可以用内置的函数 `len`；同时支持 切片 操作，因此，子串获取很容易

### 字符串比较

- `Compare` 函数
- `EqualFold` 函数

`Compare` 函数, `func Compare(a, b string) int`

用于比较两个字符串的大小，如果两个字符串相等，返回为 0。如果 a 小于 b ，返回 -1 ，反之返回 1 。不推荐使用这个函数，直接使用 `==` `!=` `>` `<` `>=` `<=` 等一系列运算符更加直观

`EqualFold` 函数，计算 s 与 t 忽略字母大小写后是否相等, `func EqualFold(s, t string) bool`

```
package main

import (
	"fmt"
	"strings"
)

func main() {
	a := "test1"
	b := "test2"
	fmt.Println(strings.Compare(a, b))
}
```

### 是否存在某个字符或子串

- `func Contains(s, substr string) bool`, 子串 substr 在 s 中，返回 true
- `func ContainsAny(s, chars string) bool`, chars 中任何一个 Unicode 代码点在 s 中，返回 true
- `func ContainsRune(s string, r rune) bool`, Unicode 代码点 r 在 s 中，返回 true


### 子串出现次数--字符串匹配

在数据结构与算法中，会讲解以下字符串匹配算法：
- 朴素匹配算法
- KMP 算法
- Rabin-Karp 算法
- Boyer-Moore 算法

在 Go 中，查找子串出现次数即字符串模式匹配，实现的是 `Rabin-Karp 算法`

`func Count(s, sep string) int`, Count 是计算子串在字符串中出现的次数

### 字符串分割为`[]string`

- Fields
- FieldsFunc
- Split
- SplitAfter
- SplitN
- SplitAfterN


`func Fields(s string) []string`, Fields 用一个或多个连续的空格分隔字符串 s，返回子字符串的数组（slice）。如果字符串 s 只包含空格，则返回空列表 ([]string 的长度为 0）

由于是用空格分隔，因此结果中不会含有空格或空子字符串

```
fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
```

`func FieldsFunc(s string, f func(rune) bool) []string`, 我们可以通过实现一个回调函数来指定分隔字符串 s 的字符


```
func Split(s, sep string) []string { return genSplit(s, sep, 0, -1) }
func SplitAfter(s, sep string) []string { return genSplit(s, sep, len(sep), -1) }
func SplitN(s, sep string, n int) []string { return genSplit(s, sep, 0, n) }
func SplitAfterN(s, sep string, n int) []string { return genSplit(s, sep, len(sep), n) }
```

这四个函数都是通过 sep 进行分割，返回`[]string`。如果 sep 为空，相当于分成一个个的 UTF-8 字符，如 `Split("abc","")`，得到的是`[a b c]`

Split 会将 s 中的 sep 去掉，而 SplitAfter 会保留 sep

带 n 的方法可以通过最后一个参数 n 控制返回的结果中的 slice 中的元素个数，当 `n < 0` 时，返回所有的子字符串；当 `n == 0` 时，返回的结果是 nil；当 `n > 0` 时，表示返回的 slice 中最多只有 n 个元素，其中，最后一个元素不会分割

### 字符串是否有某个前缀或后缀

```
// s 中是否以 prefix 开始
func HasPrefix(s, prefix string) bool {
  return len(s) >= len(prefix) && s[0:len(prefix)] == prefix
}
// s 中是否以 suffix 结尾
func HasSuffix(s, suffix string) bool {
  return len(s) >= len(suffix) && s[len(s)-len(suffix):] == suffix
}
```

如果 prefix 或 suffix 为 "" , 返回值总是 true

```
fmt.Println(strings.HasPrefix("Gopher", "Go"))
fmt.Println(strings.HasPrefix("Gopher", "C"))
fmt.Println(strings.HasPrefix("Gopher", ""))
fmt.Println(strings.HasSuffix("Amigo", "go"))
fmt.Println(strings.HasSuffix("Amigo", "Ami"))
fmt.Println(strings.HasSuffix("Amigo", ""))
```

### 字符或子串在字符串中出现的位置

```
// 在 s 中查找 sep 的第一次出现，返回第一次出现的索引
func Index(s, sep string) int

// 在 s 中查找字节 c 的第一次出现，返回第一次出现的索引
func IndexByte(s string, c byte) int

// chars 中任何一个 Unicode 代码点在 s 中首次出现的位置
func IndexAny(s, chars string) int

// 查找字符 c 在 s 中第一次出现的位置，其中 c 满足 f(c) 返回 true
func IndexFunc(s string, f func(rune) bool) int

// Unicode 代码点 r 在 s 中第一次出现的位置
func IndexRune(s string, r rune) int

// 有三个对应的查找最后一次出现的位置
func LastIndex(s, sep string) int
func LastIndexByte(s string, c byte) int
func LastIndexAny(s, chars string) int
func LastIndexFunc(s string, f func(rune) bool) int
```

### 字符串 `JOIN` 操作

将字符串数组（或 slice）连接起来可以通过 `Join` 实现:

`func Join(a []string, sep string) string`

```
fmt.Println(strings.Join([]string{"name=xxx", "age=xx"}, "&"))
```

### 字符串重复几次

`func Repeat(s string, count int) string`

将 s 重复 count 次，如果 count 为负数或返回值长度 len(s)*count 超出 string 上限会导致 panic

```
fmt.Println("ba" + strings.Repeat("na", 2))
```

### 字符替换

`func Map(mapping func(rune) rune, s string) string`

Map 函数，将 s 的每一个字符按照 mapping 的规则做映射替换，如果 mapping 返回值 <0 ，则舍弃该字符。该方法只能对每一个字符做处理，但处理方式很灵活，可以方便的过滤，筛选汉字等

```
mapping := func(r rune) rune {
    switch {
    case r >= 'A' && r <= 'Z': // 大写字母转小写
        return r + 32
    case r >= 'a' && r <= 'z': // 小写字母不处理
        return r
    case unicode.Is(unicode.Han, r): // 汉字换行
        return '\n'
    }
    return -1 // 过滤所有非字母、汉字的字符
}
fmt.Println(strings.Map(mapping, "Hello你#￥%……\n（'World\n,好Hello^(&(*界gopher..."))
```

### 字符串子串替换

进行字符串替换时，考虑到性能问题，能不用正则尽量别用，应该用这里的函数

- `func Replace(s, old, new string, n int) string`, 用 new 替换 s 中的 old，一共替换 n 个, 如果 n < 0，则不限制替换次数，即全部替换
- `func ReplaceAll(s, old, new string) string`, 该函数内部直接调用了函数 `Replace(s, old, new , -1)`

### 大小写转换

- `func ToLower(s string) string`
- `func ToLowerSpecial(c unicode.SpecialCase, s string) string`
- `func ToUpper(s string) string`
- `func ToUpperSpecial(c unicode.SpecialCase, s string) string`

大小写转换包含了 4 个相关函数，`ToLower`,`ToUpper` 用于大小写转换。`ToLowerSpecial`,`ToUpperSpecial` 可以转换特殊字符的大小写

### 标题处理

- `func Title(s string) string`
- `func ToTitle(s string) string`
- `func ToTitleSpecial(c unicode.SpecialCase, s string) string`

标题处理包含 3 个相关函数，其中 `Title` 会将 s 每个单词的首字母大写，不处理该单词的后续字符。`ToTitle` 将 s 的每个字母大写。`ToTitleSpecial` 将 s 的每个字母大写，并且会将一些特殊字母转换为其对应的特殊大写字母

### 修剪

- `func Trim(s string, cutset string) string`, 将 s 左侧和右侧中匹配 cutset 中的任一字符的字符去掉
- `func TrimLeft(s string, cutset string) string`, 将 s 左侧的匹配 cutset 中的任一字符的字符去掉
- `func TrimRight(s string, cutset string) string`, 将 s 右侧的匹配 cutset 中的任一字符的字符去掉
- `func TrimSpace(s string) string`, 将 s 左侧和右侧的间隔符去掉。常见间隔符包括：'\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL)
- `func TrimFunc(s string, f func(rune) bool) string`, 将 s 左侧和右侧的匹配 f 的字符去掉


----------------------------------------------------------

## bytes 包

该包定义了一些操作 byte slice 的便利操作。因为字符串可以表示为 `[]byte`，因此，bytes 包定义的函数、方法等和 strings 包很类似

为了方便，会称呼 `[]byte` 为字节数组

----------------------------------------------

## strconv 包

字符串和基本数据类型之间转换，这里的基本数据类型包括：布尔、整型和浮点型等

### 字符串转为整型

- ParseInt
- ParseUint
- Atoi

```
func ParseInt(s string, base int, bitSize int) (i int64, err error)
func ParseUint(s string, base int, bitSize int) (n uint64, err error)
func Atoi(s string) (i int, err error), 只能转换包含整数的字符串，对浮点型的字符串无效
```

### 整型转为字符串

```
func FormatUint(i uint64, base int) string    // 无符号整型转字符串
func FormatInt(i int64, base int) string    // 有符号整型转字符串
func Itoa(i int) string
```

### 重点

- func Atoi(s string) (i int, err error) , string=> int
- func Itoa(i int) string , int=> string


----------------------------------------------------

## string 和 []byte 的转换

```
//string to []byte
s1 := "ifcalm"
b := []byte(s1)

//[]byte to string
s2 := string(b)
```

**因为无法修改`string`中的某个字符，需要粒度小到操作一个字符时，用`[]byte`**

----------------------------------------------------

## int 转 string 的几种方式对比

- `strconv.Itoa()`函数的参数是一个整型数字，它可以将数字转换成对应的字符串类型的数字
- `string()`函数的参数若是一个整型数字，它将该整型数字转换成`ASCII`码值等于该整形数字的字符。`string()`函数是Go语言的内置函数，不需要导入任何包

```
package main

import "fmt"

func main() {
	n := 97
	str := string(n)
	fmt.Printf("%s, %T", str, str)
}
```
运行结果为: `a, string`

因为`ASCII`码值为97对应的字符是`a`，所以`string(97)`的结果是`a`


```
package main

import (
	"fmt"
	"strconv"
)

func main() {
	n := 97
	str := strconv.Itoa(n)
	fmt.Printf("%s, %T", str, str)
}
```
运行结果为: `97, string`, `strconv.Itoa()`函数它可以将数字转换成对应的字符串类型的数字


-------------------------------------------------

### 算法常用字符串函数

- strings.Replace(), 字符串替换
- strings.Fields(), strings.Split(), 字符串分割为字符串切片
- strings.Join(), 把字符串切片拼接为字符串
- strings.Contains(str, substr), 判断str中是否包含substr


--------------------------------------------

### 进制转换函数

```
var v int64 = 12     //默认10进制
s2 := strconv.FormatInt(v, 2)   //10 转2进制
s8 := strconv.FormatInt(v, 8)   //10 转8进制
s10 := strconv.FormatInt(v, 10) //10 转10进制
s16 := strconv.FormatInt(v, 16) //10 转16进制


var sv = "11"
fmt.Println(strconv.ParseInt(sv, 16, 32)) // 16 转 10
fmt.Println(strconv.ParseInt(sv, 10, 32)) // 10 转 10
fmt.Println(strconv.ParseInt(sv, 8, 32))  // 8 转 10
fmt.Println(strconv.ParseInt(sv, 2, 32))  // 2 转 10
```

转换时需去掉 `0x`, `0b` 等进制标识符

-----------------------------------------------------


