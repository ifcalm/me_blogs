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

