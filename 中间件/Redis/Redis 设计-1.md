在开源界，高性能服务的典型代表就是**Nginx和Redis**。纵观这两个软件的源码，都是非常简洁高效的，也都是基于异步网络I/O机制的，所以对于要学习高性能服务的程序员或者爱好者来说，研究这两个网络服务的源码是非常有必要的


**Redis是目前最流行的键值对（key-value）数据库，以出色的性能著称**

1. Redis是基于内存的存储数据库，绝大部分的命令处理只是纯粹的内存操作，内存的读写速度非常快
2. Redis是单进程线程的服务（实际上一个正在运行的Redis Server肯定不止一个线程，但只有一个线程来处理网络请求），避免了不必要的上下文切换，同时不存在加锁/释放锁等同步操作
3. Redis使用多路I/O复用模型（select、poll、epoll），可以高效处理大量并发连接
4. Redis中的数据结构是专门设计的，增、删、改、查等操作相对简单

## Redis简介

Redis（REmote DIctionary Server）是一个使用ANSI C编写的、开源的、支持网络的、基于内存的、可选持久化的键值对存储系统

Redis是目前最流行的键值对存储系统

**Redis在互联网数据存储方面应用广泛，主要具有以下优点**

1. Redis是内存型的数据库，也就是说Redis中的key-value对是存储在内存中的，因而效率比磁盘型的快
2. Redis的工作模式为单线程，不需要线程间的同步操作。Redis采用单线程主要因为其瓶颈在内存和带宽上，而不是CPU
3. Redis中key-value的value不仅可以是字符串，也可以是复杂的数据类型，如链表、集合、散列表等
4. Redis支持数据持久化，可以采用RDB、AOF、RDB&AOF三种方案。计算机重启后可以在磁盘中进行数据恢复
5. Redis支持主从结构，可以利用从实例进行数据备份


