## 标题
在想要设置为标题的文字前面加 `#` 即可，
标准语法需要在 `#` 后面加一个空格
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
```

## 字体
要加粗的文字左右分别用两个 `*` 号括起来-(**加粗**)

要倾斜的文字左右分别用一个 `*` 号括起来-(*倾斜*)

要倾斜加粗的文字左右两别分别用三个 `*` 号括起来-(***加粗倾斜***)

要加删除线的文字左右两别用两个 `~` 号括起来-(~~删除线~~)
```
**加粗文字**
*倾斜文字*
***倾斜加粗文字***
~~有删除线文字~~
```

## 引用
在引用的内容前加 `>`

> 引用内容
>> 二次引用

```
> 引用内容
>> 引用内容 (引用嵌套)
```

## 分割线
三个或者三个以上的 `*` 

```
***
*****
```
***

## 图片
插入图片语法：

`![图片描述](图片地址 "图片 title")`

示例：
```
![leetcode](https://github.com/ifcalm/me_blogs/blob/master/develop_tools/images/markdown_1.png "leetcode题解群聊")
```
效果如下图所示：

![leetcode](https://github.com/ifcalm/me_blogs/blob/master/develop_tools/images/markdown_1.png "leetcode题解群聊")


## 超链接
超链接语法：

`[超链接名称](超链接地址 "超链接 title")`

title 可以省略

**示例：**
```
[google](google.com)

[Stack Onerflow](https://stackoverflow.com/ "技术交流论坛")
```

**效果入下：**

+ [google](google.com)

+ [Stack Onerflow](https://stackoverflow.com/ "技术交流论坛")


## 列表
**无序列表**

无序列表需要在列出的项前面加一个 `-`或 `+` 或`*`，`-` `+` `*` 与内容之间需要有一个空格

示例：

```
- 1
- 2
- 3

+ x
+ y
+ z

* A
* B
* C
```

效果如下：
- 1
- 2
- 3

+ x
+ y
+ z

* A
* B
* C


**有序列表**

语法：

`数字加点` -序号跟内容之间需要有空格

示例：
```
1. ABC
2. XYZ
3. abc
4. xyz
```

效果如下：

1. ABC
2. XYZ
3. abc
4. xyz

**列表嵌套**

语法：

`上一级和下一级直接敲三个空格`

示例：
```
+ 一级目录
   + 二级目录
   + 二级目录
   + 二级目录

1. 一级有序目录
   1. 二级目录
   2. 二级目录
   3. 二级目录
```
效果如下：

+ 一级目录
   + 二级目录
   + 二级目录
   + 二级目录

1. 一级有序目录
   1. 二级目录
   2. 二级目录
   3. 二级目录


## 表格
语法：
```
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右

示例：
姓名|年龄|爱好
--|:--:|--:
lss|24|code
lfq|50|work
qxm|47|say
lqq|23|cool
```

效果如下：

姓名|年龄|爱好
--|:--:|--:
lss|24|code
lfq|50|work
qxm|47|say
lqq|23|cool

## 代码
**单行代码：** 左右两别分别用一个 反引号 括起来

**代码块：** 代码块之间分别用 三个 反引号 括起来，并且两别的反引号 单独占一行

示例：
```
`单行代码`

    ```
    多行代码
    ```
```
