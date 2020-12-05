## Java 基础知识点---语法类

### 第一个程序

HelloWorld.java

```
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("hello world");
    }
}
```

Java 开发工具包 (Java Development Kit), 简称 JDK


![](img/java_1.PNG)

### Java 开发编辑器

IntellJ IDEA

### 程序的移植

### Java 中的关键字

Java 语言中有一些具有特殊用途的词被称为关键字

- abstract
- boolean
- braek
- byte
- case
- catch
- char
- class
- continue
- default
- do
- double
- else
- extends
- false
- final
- finally
- float
- for
- if
- implements
- import
- instanceof
- in
- interface
- long
- native
- new
- null
- package
- private
- protected
- public
- return
- short
- static
- super
- switch
- synchronized
- this
- throw
- throws
- transient
- true
- try
- void
- volatile
- while

java 关键字是区分大小写的

### Java 标识符

标识符就是用于给 java 程序中 变量，类，方法等命名的符号

```
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("hello world");
    }
}
```

如上面程序所示, HelloWorld 就是一个类的标识符

#### 标识符规则

- 标识符可以由字母、数字、下划线（_）、美元符（$）组成, 不能以数字开头
- 标识符不能是 Java 关键字和保留字
- 标识符是严格区分大小写的
- 标识符的命名最好能反映出其作用，做到见名知意


### 变量

在 java 中通过三个元素描述变量:
- 变量类型
- 变量名
- 变量值

```
public class HelloWorld {
    public static void main(String[] args){
        String love = "hello java";  //定义变量
        System.out.println("hello world " + love);
        love = "java and golang";   //为变量重新赋值
        System.out.println("Hi " + love);
    }
}
```

从上面程序可以看出 java 程序使用 `+` 进行字符串拼接

### java 变量命名规则

- 变量名由多单词组成时，第一个单词的首字母小写，其后单词的首字母大写，俗称骆驼式命名法
- 变量命名时，尽量简短且能清楚的表达变量的作用，做到见名知意
- Java 变量名的长度没有限制，但 Java 语言是区分大小写的，所以 price 和 Price 是两个完全不同的变量


### Java 中的数据类型

**Java 语言是一种强类型语言。通俗点说就是，在 Java 中存储的数据都是有类型的，而且必须在编译时就确定其类型**

数据类型, 分为基本数据类型和引用数据类型:

- 整数类型
- 浮点类型
- 字符型
- 布尔型
- 类
- 接口
- 数组

在 Java 的领域里，**基本数据类型变量存的是数据本身，而引用类型变量存的是保存数据的空间地址**


### Java 中变量的使用规则

- Java 中的变量需要先声明后使用
- 变量使用时，可以声明变量的同时进行初始化
- main 方法中定义的变量必须先赋值，然后才能输出


### Java 中的自动类型转换

在 Java 程序中，不同的基本数据类型的数据之间经常需要进行相互转换

- 目标类型能与源类型兼容，如 `double` 型兼容 `int` 型，但是 `char` 型不能兼容 `int` 型
- 目标类型大于源类型，如 `double` 类型长度为 8 字节， `int` 类型为 4 字节，因此 `double` 类型的变量里直接可以存放 `int` 类型的数据，但反过来就不可以了

```
public class HelloWorld{
    public static void main(String[] args) {
		double avg1=78.5;
		int rise=5;
		double avg2=avg1+rise;
		System.out.println("考试平均分："+avg1);
		System.out.println("调整后的平均分："+avg2);
	}
}
```

### Java 中的强制类型转换

当程序中需要将 double 型变量的值赋给一个 int 型变量，该如何实现呢？显然，这种转换是不会自动进行的！因为 int 型的存储范围比 double 型的小。此时就需要通过强制类型转换来实现了。

```
public class HelloWorld{
    public static void main(String[] args) {
		double heightAvg1=176.2;
		int heightAvg2=(int)heightAvg1;
		System.out.println(heightAvg1);
		System.out.println(heightAvg2);
	}
}
```

`int heightAvg2=(int)heightAvg1;`, 强制类型转换可能会造成数据的丢失


### Java 常量的应用

所谓常量，我们可以理解为是一种特殊的变量，它的值被设定后，在程序运行过程中不允许改变

语法：`final 常量名 = 值;`

```
public class HelloWorld{
    public static void main(String[] args) {
		final char MAN_SEX = '男';
		final char WOMAN_SEX = '女';
		System.out.println(MAN_SEX);
		System.out.println(WOMAN_SEX);
	}
}
```

程序中使用常量可以提高代码的可维护性。例如，在项目开发时，我们需要指定用户的性别，此时可以定义一个常量 SEX，赋值为 "男"，在需要指定用户性别的地方直接调用此常量即可，避免了由于用户的不规范赋值导致程序出错的情况

**常量名一般使用大写字符**


