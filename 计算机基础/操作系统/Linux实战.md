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

- cat   文本内容显示到终端
- head  默认查看文件开头10行, `head -5 a.txt` 查看文件前五行
- tail  查看文件结尾，常用参数 `-f`, `tail -f a.txt` 可以同步查看到信息的变化, 常用于调试时查看日志的变化信息
- wc    统计文件内容信息, 常用参数 `-l`，`wc -l a.txt` 查看文件的行数，根据行数可以确定使用 `head` 和 `tail` 来进行文件查看
- more
- less


### 打包压缩和解压缩

`tar` 打包命令, 常用参数有:
- c 打包
- x 解包
- f 指定操作类型为文件
  

`tar` 将文件打包, 打包后可以进行压缩存储, 压缩命令有:
- gzip, 经常使用的扩展名是 .tar.gz
- bzip2, 经常使用的扩展名是 .tar.bz2

---------------------------------

1. 打包
   
   `tar cf test1/a.tar test4`, 把 test4 目录打包为一个文件, 命令为a.tar, 参数 `f` 表示打包为文件, 参数 `c` 表示打包

2. 打包并压缩
   
   `tar czf test1/a.tar.gz /etc`, 把 目录 /etc 打包并压缩为 a.tar.gz 文件, 参数 `z` 表示压缩为 gzip 格式, 为了便于区分, 我们把只打包的文件后缀命名为 `.tar`, 打包并压缩的文件后缀命名为 `.tar.gz`

   `tar cjf test1/a.tar.bz2 /etc`, 把目录 /etc 打包并压缩为 a.tar.bz2 文件, 参数 `j` 表示压缩为 bzip2 格式

3. 解压
   
   `tar xf a.tar -C /root`, 把 a.tar 解包到 /root 目录下, 参数 `x` 为解包操作

   `tar xzf a.tar.gz -C /root`, 解压 .tar.gz 的包（gzip 格式）

   `tar xjf a.tar.bz2 -C /root`, 解压 .tar.bz2 格式的包 (bzip2 格式)
   


### Vim 编辑器

Vim 的四种模式:
- 正常模式
- 插入模式
- 命令模式
- 可视模式

-------------------
#### Vim 正常模式常用操作


`i` 进入插入模式, 可以进行输入内容

`v` 进入可视模式

`h g k l` 光标 上下左右 移动

`yy` 复制, `p` 粘贴

`3yy` 复制光标向下的3行

`y$` 复制本行光标到结尾的内容

`dd` 截切一整行, `p` 粘贴

`5dd` 截切5 行

`d$` 截切本行光标到结尾的内容

`u` 撤销上一步操作

`ctrl + r` 撤销 **撤销操作**，用于 `u` 操作使用过多的回撤

`x` 单个字符删除

`r` 替换当前光标所在位置的内容，（需要输入替换的内容）

`:set nu` 显示行号

`13 G` 光标快速移动到第 13 行

`g` 移动到第一行（首行）

`G` 移动到末尾（最后一行）

`^` 移动到一行的开头

`$` 移动到一行的末尾


----------------------------

#### Vim 命令模式

`esc` 退出输入模式之后，会进入的正常模式， 输入 `:` 后进入命令模式，也叫做 末行模式

`:w` 保存文件

`:w /home/lss/test3/vim.txt`  将内容保存到指定文件

`:q!` 不保存退出

`:! + linux命令`, `:! ip addr`, `:! pwd`

`/x` 查找文本中的 x

`:s/old/new` 查找到 old 之后 替换为 new, 只针对光标所在行进行搜索替换

`:%s/old/new` 针对全局文本进行搜索替换, 只替换一个

`:%s/old/new/g`, `g` 表示 global , 全局替换的意思, 会把文件中所有 old 字符 替换为 new 字符 

`:3,5s/old/new/g` 对3-5 行的内容进行全部替换

`:set nu` 显示行号, `:set nonu` 取消显示行号

vim 软件所在的配置文件 `/etc/vimrc` , 相当于修改软件的配置，一次修改，全局生效

------------------------

#### 可视模式

用于块的重复性操作

进入可视模式的方式:
- v          字符可视模式
- V          行可视模式
- ctrl + v   块可视模式


-----------------------

### 用户与用户组管理

