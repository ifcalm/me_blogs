# Docker 基础

## 为什么要使用 Docker

* 更高效的利用系统资源

  由于容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，`Docker` 对系统资源的利用率更高。无论是应用执行速度、内存损耗或者文件存储速度，都要比传统虚拟机技术更高效。因此，相比虚拟机技术，一个相同配置的主机，往往可以运行更多数量的应用。

* 更快速的启动时间

  传统的虚拟机技术启动应用服务往往需要数分钟，而 `Docker` 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。

* 一致的运行环境

  发过程中一个常见的问题是环境一致性问题。由于开发环境、测试环境、生产环境不一致，导致有些 bug 并未在开发过程中被发现。而 Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性

* 持续交付和部署
* 更轻松的迁移
* 更轻松的维护和扩展


## 镜像，容器，仓库

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样

每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 容器存储层

镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry 就是这样的服务。

一个 Docker Registry 中可以包含多个 仓库（Repository）；每个仓库可以包含多个 标签（Tag），每个标签对应一个镜像。

通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 <仓库名>:<标签> 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签

## 安装 Docker
Docker 分为 CE 和 EE 两大版本。CE 即社区版（免费，支持周期 7 个月），EE 即企业版，强调安全，付费使用，支持周期 24 个月

#### 使用脚本自动安装
```
curl -fsSL get.docker.com -o get-docker.sh

sudo sh get-docker.sh --mirror Aliyun

sudo sh get-docker.sh --mirror AzureChinaCloud
```

#### 启动 Docker CE
```
systemctl enable docker
systemctl start docker
```

#### 测试 Docker 是否安装正确
```
docker run hello-world
```

## 使用镜像

#### 获取镜像
```
docker pull

docker pull --help 查看帮助文档

docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]

docker pull ubuntu:18.04
```

#### 运行

有了镜像后，我们就能够以这个镜像为基础启动并运行一个容器。以上面的 ubuntu:18.04 为例，如果我们打算启动里面的 bash 并且进行交互式操作的话

```
docker run -it --rm ubuntu:18.04 bash
```

`docker run` 就是运行容器的命令

`-it`：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终端。

`--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 --rm 可以避免浪费空间。

`ubuntu:18.04`：这是指用 ubuntu:18.04 镜像为基础来启动容器。

`bash`：放在镜像名后的是 命令，这里我们希望有个交互式 Shell，因此用的是 bash

通过 `exit` 来退出这个容器


#### 列出镜像

要想列出已经下载下来的镜像，可以使用：
```
docker image ls

docker images

docker image ls -a

docker image ls ubuntu  //根据仓库名列出镜像
```

#### 删除本地镜像
```
docker image rm

docker image rm [选项] <镜像1> [<镜像2> ...]
```

#### docker commit

`docker commit` 命令除了学习之外，还有一些特殊的应用场合，比如被入侵后保存现场等。但是，不要使用 `docker commit` 定制镜像，定制镜像应该使用 `Dockerfile` 来完成

## 使用 Dockerfile 定制镜像

Dockerfile 是一个文本文件，其内包含了一条条的 指令(Instruction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建

```
mkdir mynginx
cd mynginx
touch Dockerfile
```

内容为：
```
FROM nginx
RUN echo '<h1>Hello, Docker</h2>' > /usr/share/nginx/html/index.html
```
FROM 用于指定基础镜像
RUN 勇于执行命令

#### 构建镜像
```
docker build -t nginx:v3
```

#### Dockerfile 指令详解

* COPY 复制文件
  格式：
  ```
  
  ```
  `COPY` 指令将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置。比如：