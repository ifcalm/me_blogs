## yaml

YAML 是 `YAML Ain't a Markup Language` YAML 不是一种标记语言的递归缩写。在开发的这种语言时，YAML 的意思其实是：`Yet Another Markup Language` 仍是一种标记语言

### 基本语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进只允许使用空格
- 缩进的空格不重要，只要相同层级的元素对其即可
- `#` 表示注释


### yaml支持的数据类型

1. 对象: 键值对的集合
2. 数组
3. 纯量: 单个的 不可在分的值


#### 对象

对象键值对使用冒号结构表示 `key: value`, 冒号后面要加一个空格

也可以使用 `key: {key1: value1, key2: value2, ...}`

还可以使用缩进表示层级关系:
```
key:
    key1: value1
    key2: value2
```

#### 数组

以 `-` 开头的行表示构成一个数组

```
- A
- B
- C
```

多维数组:
```
- 
 - A
 - B
 - C
```

#### 复合结构

数组和对象可以构成复合结构:

```
languages:
    - Golang
    - Python
    - Java
    - C++
websites:
    Golang: golang.org
    Python: python.org
    Java: java.org
    C++: gcc.org
```

转换为 json 如下:
```
{
    languages: ["Golang", "Python", "Java", "C++"]
    websites: {
        "Golang": "golang.org",
        "Python": "python.org",
        "Java": "java.org",
        "C++": "gcc.org"
    }
}
```

#### 纯量

纯量是最基本的，不可在分的值，包括:
- 字符串
- 布尔值
- 整数
- 浮点数
- null
- 时间
- 日期


```
boolean:
    - true
    - false
float:
    - 3.14
    - 6.85e+5  #可以使用科学计数法
int:
    - 123
    - 0b0101   #二进制表示
null:
    node: 'node'
    parent: ~   #使用 ~ 表示null
string:
    - 要开心
    - 'Hello World'  #可以使用双引号或者单引号包裹特殊字符
date:
    - 2020-11-09  # 日期必须使用 yyyy-MM-dd 格式
datetime:
    - 2020-11-09T15:02:31+08:00
```

### 引用

- `&` 锚点
- `*` 别名


