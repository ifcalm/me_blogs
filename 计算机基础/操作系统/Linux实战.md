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


**在Linux 中一切皆文件，所以对文件的操作非常重要**

### 显示当前的目录名称

使用 `pwd` 命令

### 更改当前的操作目录

`cd` 命令

ep: `cd /path/file`

### 查看当前目录下的文件

`ls` 命令

ep: `ls -l`

常用参数:

- -l , 长格式显示文件
- -a , 显示隐藏文件
- -r , 逆序显示文件
- -t , 按照时间顺序显示
- -R , 递归显示   ep: `ls -R`

同时查询多个目录: `ls /dev /etc`


### 如何切换用户

`su root` 从普通用户切换到 root 用户

`su lss` 切换到 lss 用户


### 命令解释器 bash


### 创建和删除目录

`mkdir` 创建目录

ep: `mkdir test`

创建多个目录: `mkdir test1 test2 test3`

创建多级目录: `mkdir -p a/b/c/d`

查看多级目录: `ls -R a`

删除目录: `rmdir d`, `rmdir` 命令只能删除空目录

删除非空目录: `rm -rf a` 或者 `rm -r a` 递归删除目录，并会进行提示

### 复制和移动目录

复制命令: `cp`

`cp test1 test2` cp 命令只能复制文件, 复制目录时需要加 `-r` 参数: `cp -r test1 test2`

`cp -v test1/a.txt test2`, `-v` 参数可以显示复制过程

`cp -p test1/a.txt test3`, `-p` 参数可以保留源文件的时间和用户操作权限等信息

`cp -a test1/a.txt test3`, `-a` 参数可以复制更多的信息，包括文件的属主等，是 `-p` 参数的加强版


`mv` 命令的两个功能:
- 移动文件目录
- 重命名

`mv test test4`, 把 test 目录更名为 test4. (同级目录更名)

`mv test1/a.txt test2`, 把 a.txt 文件移动到 test2 目录下

`mv * /test4`, 把当前目录所有内容移动到 /test4 目录下


### 新建空白文件

`touch a.txt`


### 通配符

`*`, 任意多个字符匹配

`?`, 单个字符匹配


### Linux 文件查看


