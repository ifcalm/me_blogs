## Web开发简介

因为Go的`net/http`包提供了基础的路由函数组合与丰富的功能函数，所以在开源社区里流行一种用Go编写API不需要框架的观点，在我们看来，如果你的项目的路由在个位数、URI固定且不通过URI来传递参数，那么确实使用官方库也就足够了。但在复杂场景下，官方的HTTP库还是有些力有不逮

```
GET    /card/:id
POST   /card/:id
DELETE /card/:id
GET    /card/:id/name
```

Go的Web框架大致可以分为两类:
- Router框架
- MVC类框架

简单来说，只要你的路由带有参数，并且这个项目的API数目超过了10，就尽量不要使用`net/http`中默认的路由。在Go开源界应用最广泛的路由器是`httprouter`，很多开源的路由器框架都是基于`httprouter`进行一定程度的改造的成果

开源界有这样几种框架，第一种是对httprouter进行简单的封装，然后提供定制的中间件和一些简单的小工具集成比如**gin**，主打轻量、易学、高性能。第二种是借鉴其他语言编程风格的一些MVC类框架，如**beego**，方便从其他语言迁移过来的程序员快速上手，快速开发


## 请求路由

在常见的Web框架中，路由器是必备的组件。Go语言圈子里路由器也时常称为**http的多路复用器**

**REST风格是几年前刮起的API设计风潮，在REST风格中除GET和POST之外，还使用了HTTP协议定义的几种其他标准化语义**

REST风格的API重度依赖请求路径，会将很多参数放在请求URI中。除此之外还会使用很多并不常见的HTTP状态码

```
const (
    MethodGet      = "GET"
    MethodHead     = "HEAD"
    MethodPost     = "POST"
    MethodPut      = "PUT"
    MethodPatch    = "PATCH"
    MethodDelete   = "DELETE"
    MethondConnect = "CONNECT"
    MethodOptions  = "OPTIONS"
    MethodTrace    = "TRACE"
)
```

### httprouter

较流行的开源Go Web框架大多使用httprouter，或是基于httprouter的变种对路由进行支持

httprouter考虑到字典树的深度，在初始化时会对参数的数量进行限制，所以在路由中的参数数目不能超过255，否则会导致httprouter无法识别后续的参数。不过，在这一点上也不用考虑太多，毕竟URI是设计给人来看的，相信没有夸张的URI能在一条路径中带有200个以上的参数


## 中间件

### 使用中间件剥离非业务逻辑

每一个Web框架都会有对应的中间件组件

## 请求校验

实际上这是一个与语言无关的场景，需要进行字段校验的情况有很多，Web系统的Form或JSON提交只是一个典型的例子

### 用请求校验器解放体力劳动

## Database 和数据库打交道

Go官方提供了database/sql包来使用户进行和数据库打交道的工作，实际上database/sql库只是提供了一套操作数据库的接口和规范，例如抽象好的SQL预处理、连接池管理、数据绑定、事务、错误处理等。官方并没有提供具体某种数据库实现的协议支持

和具体的数据库（如MySQL）打交道，还需要再引入MySQL的驱动

### 提高生产效率的ORM和SQL Builder

在Web开发领域常常提到的ORM是什么？对象关系映射（Object Relational Mapping，简称ORM、O/RM或O/R映射）是一种程序设计技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。从效果上说，它其实是创建了一个可在编程语言里使用的**虚拟对象数据库**

**最为常见的ORM实际上做的是从数据库数据到程序的类或结构体这样的映射**

实际上很多语言的ORM只要把你的类或结构体定义好，再用特定的语法将结构体之间的一对一或者一对多关系表达出来，那么任务就完成了。然后你就可以对这些映射好了数据库表的对象进行各种操作，如增删改查

## 服务流量限制




