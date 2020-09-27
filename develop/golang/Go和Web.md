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




