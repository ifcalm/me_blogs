## Linux 常用功能总结

### Linux 常见目录介绍

- /                 根目录
- /root             root用户的家目录
- /home/username    普通用户的家目录
- /etc              配置文件目录
- /bin              命令目录
- /sbin             管理命令目录
- /usr/bin          系统预装的其他命令
- /usr/sbin         系统预装的其他命令


### 帮助命令 man, help, info

1. man 是 manual 的缩写
   
   man 的用法: `man ls`, `man 1 passwd`, `man 5 passwd`

2. help 帮助命令:

    shell 自带的命令称为**内部命令**，其他的是**外部命令**

    内部命令使用 help 帮助: `help cd`, 外部命令使用 help 帮助: `ls --help`

3. info 帮助: info 帮助比 help 更详细，作为 help 的补充

    info 用法: `info ls`

### 如何判断一个命令为内部或外部命令

`type ls`, 输出: ls is aliased to `ls --color=auto'

`type cd`, 输出: cd is a shell builtin


