## 什么事docker

**Docker：**Go 语言开发，基于 linux 内核，对进程进行封装隔离，操作系统层面的虚拟化技术。



文件系统、互联网、进程隔离。

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220419224150684.png" alt="image-20220419224150684" style="zoom:50%;" />



## 为什么用 Docker

#### 更高效利用系统资源

#### 更快速的启动时间

直接运行在宿主内核，无需启动完整的虚拟环境。

#### 一致的运行时间

Docker 镜像提供了除内核外，完整的运行环境

#### 持续交付和部署

Dockerfile 进行镜像构建，持续集成系统进行测试集成测试，结合持续部署系统进行快速部署。

#### 更轻松的迁移

Docker 可以在物理机、虚拟机、公有云、私有云、甚至云笔记上运行。

#### 更轻松的维护和扩展

Docker 使用分层存储和镜像技术，使重复部分复用更为容易。



<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220419224530150.png" alt="image-20220419224530150" style="zoom:50%;" />

## 基本概念

#### 镜像（image)

>  补充：操作系统分为内核和用户空间，启动后挂载 root 文件系统，为其提供用户空间支持。

*Docker 镜像相当于一个 root 文件系统。*

**特殊文件系统**：提供库、资源、配置、文件等，运行时准备的配置参数。镜像不包含动态数据，构建后不改变。

**分层存储**：每一层构建完就不会改变，上一层会在基础上做改动，即使删除了下一层的文件，也只是标记删除。



#### 容器（container）

> 容器和镜像 -》 类和实例。

**容器**实质是进程，拥有自己独立的命名空间，容器可以拥有自己独立的 root 文件系统、网络、自己的进程空间。

**特性**：容器也是分层的，以镜像为基础，在其上创建一个当前容器的存储层（容器存储层）。容器消亡，容器存储层也消亡。

容器不应该向存储层写入数据，应该写入数据卷、绑定的宿主目录。



#### 仓库（repository）

> 集中分发、存储镜像的服务，Docker Registry

**Docker Registry**：包含多个仓库，每个仓库包含多个标签（Tag）；每个标签对应一个镜像。

<仓库名>:<标签>

常用 Docker Registry 是 Docker hub，国内网易云镜像服务、阿里云镜像服务。



## 使用镜像

#### 获取镜像

```shell
docker pull nignx:latest
```

 **容器运行：**

```shell
docker run -it --rm ubuntu:18.04 bash
```

* -it ：-i 交互式，-t 终端
* --rm：容器退出后随即将其删除
* bash：希望有个交互式命令

#### 列出镜像

```shell
docker iamges ls
```

因为分层存储，实际镜像大小可能会更小。

```shell
docker system df
```

1. **虚悬镜像**

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220419233225021.png" alt="image-20220419233225021" style="zoom:50%;" />

由于 docker pull 或 docker build，镜像名与旧镜像名相同，旧镜像名被置为 none。

查看：

```shell
docker image ls -f dangling=true
```

删除：

```shell
$ docker image prune
```

2. **中间层镜像**

   顶层镜像所依赖的镜像

```shell
$ docker image ls -a
```

3. **列出部分镜像**

```shell
$ docker image ls ubuntu
$ docker image ls -f since=mongo:3.2 # 列出之后的镜像 -f 是 filter 的意思
$ docker image ls -f label=com.example.version=0.1 # 使用 label 来过滤
```

4. **特定格式显示**

```shell	
$ docker image ls -q # 只显示容器ID
```

更强大使用 GO 模板语法。



## 删除本地镜像

```shell
docker image rm 3241
```

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220419234512023.png" alt="image-20220419234512023" style="zoom:50%;" />

删除分为 untagged 和 deleted，因为有依赖存在，如果有上层依赖下层镜像，那么不会真的被删除。

**批量删除**：

删除所有仓库名为 redis 的镜像：

```shell
$ docker image rm $(docker image ls -q redis)
```
删除所有在 mogo 之后的镜像：

```shell
$ docker image rm $(docker image ls -q -f before=mongo:3.2)
```



## 利用 Commit 理解镜像构成（不推荐使用）

对容器的任何文件修改都会记录到容器存储层，可以再容器存储层的基础上制作镜像。

运行一个镜像，对其内文件进行修改，然后 commit 进行制作镜像

```shell
$ docker commit \
    --author "Tao Wang <twang2218@gmail.com>" \
    --message "修改了默认网页" \
    webserver \
    nginx:v2
    
$ docker history nginx:v2 # 查看镜像内的历史记录

$ docker run --name web2 -d -p 81:80 nginx:v2 #运行新镜像
```



## 使用 Dockerfile 制作镜像

**Dockerfile **是一个文本文件，包含了一条条指令，每一条指令构建一层，因此每一条指令就是描述该层如何构建。

创建一个空白文件 Dockerfile：

```shell
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

**FROM** 指定基础镜像

> 很多优秀官方镜像可以作为基础镜像，还有一个特殊镜像 **scratch**，虚拟概念，空白的镜像。使用 GO 语言开发会用这种方式，会让体积更小

**RUN** 执行命令(每个 RUN 都会创建一次镜像)

*shell* 格式：RUN <命令>，就像直接再命令行中输入命令一样

Dockerfile:

```shell
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

多行命令：

在最后添加清理命令，删除无关内容，使体积更小

​	<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220420001307007.png" alt="image-20220420001307007" style="zoom:50%;" />



exec 格式：RUN ["可执行文件", "参数1", "参数2"]

#### 构建镜像

**在 Dockerfile 所在目录执行**

```shell
$ docker build -t nginx:v3 .
```

docker build [选项] <上下文路径/URL/->

. 指的是上下文路径

#### **镜像构建上下文**

docker build 命令构建镜像，并非在本地构建，而是在服务的。基于 C/S 设计。

docker build 命令会将指定上下文的所有文件打包上传到 Docker 引擎。

>  .dockeringnore 可以剔除该上下文目录中不需要上传的文件。（类似于 .gitignore)

当然 docker build 可以指定文件，可以不在上下文目录中

``` shell
docker build -t nginx:v4 -f ../duckfile
```

#### 其它 docker build 的用法

* **直接用 Git repo 进行构建**

  docker build 支持从 URL 构建，比如可以冲 Git repo 中构建：

  ```shell
  docker build -t hello-world https://github.com/docker-library/hello-world.git#master:amd64/hello-world
  ```

  该指令指定了构建所需的 Git repo，指定分支为 master，构建目录为 /amd64/hello-world/，然后 docker 会去 git clone 这个项目、切换到指定分支，并进入到指定目录后开始构建。

* **给定的 tar 压缩包构建**

  ```shell
  docker build http://server/context.tar.gz
  ```

* **从标准输入中读取 Dockerfile 进行构建**

  ```shell
  docker build - < Dockerfile
  或
  cat Dockerfile | docker build -
  ```

* **从标准输入中读取上下文压缩包进行构建**

  ```shell
  docker build - < context.tar.gz
  ```



## Dockerfile 指令详解

#### COPY 复制文件

```shell
COPY package.json /usr/src/app/
```

通配符（符合 GO 的 filepath.Match 规则：

```shell
COPY hom* /mydir/
COPY hom?.txt /mydir
```

改变文件所属用户和所属组：

```shell
COPY --chown=55:mygroup files* /mydir/
COPY --chown=bin files* /mydir/
COPY --chown=1 files* /mydir/
COPY --chown=10:11 files* /mydir/
```

#### ADD 更高级的复制文件

支持复制时从 url 下载和解压 tar，一般不用。

#### CMD 容器启动命令

Docker 是进程，容器启动时，需要指定运行的程序和参数。CMD 命令用于指定默认的容器主进程启动命令的。

* shell 格式：CMD <命令>
* exec 格式：CMD ["可执行文件","参数1","参数2"...]
* 参数列表格式：CMD ["参数1","参数2"...]

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425150452273.png" alt="image-20220425150452273" style="zoom:50%;" />

Docker 不是虚拟机，容器中的应用都应该在前台执行。

#### ENTRYPOINT 入口点

**场景一：让镜像变成像镜像一样使用**

```shell
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "http://myip.ipip.net" ]
```

```shell
$ docker run myip
当前 IP：61.148.226.66 来自：北京市 联通
```

```shell
$ docker run myip -i # 加 -i 打印 header 等参数
docker: Error response from daemon: invalid header field value "oci runtime error: container_linux.go:247: starting container process caused \"exec: \\\"-i\\\": executable file not found in $PATH\"\n".
```

-i 会替换 CMD，-i 不是命令因此报错

ENTRYPOINT 解决：

```shell
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "http://myip.ipip.net" ]
```

```shell
$ docker run myip -i
HTTP/1.1 200 OK
Server: nginx/1.8.0
```

**场景二：应用运行前的准备**

比如 mysql，运行前需要一些数据库配置、初始化的工作。

避免使用 root 用户启动服务，准备工作可以使用 root 账户启动服务。

这些准备工作与 CMD 无关，无论 CMD 是什么，都需要一些预处理的工作。可以写一个脚本放到 ENTRYPOINT 中执行，这个脚本会将接到的参数（<CMD>)作为命令，在脚本最后执行。比如官方镜像 redis 中这么做：

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425155203643.png" alt="image-20220425155203643" style="zoom:50%;" />

#### ENV 设置环境变量

格式有两种：

* ENV <key> <value>
* ENV <key1>=<value1> <key2>=<value2> ...

```shell
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
```

#### ARG 构建参数

格式：ARG <参数名>[=<默认值>]

ARG 与 ENV 效果相同，ARG 设置的环境变量，将来容器运行不会存在的。

可以再构建命令 docker build 中用 --build-arg <参数名>=<值> 来覆盖。

ARG 指令生效范围：

* FROM 指令之前，作用于 FROM 指令之中。

  ```shell
  # 只在 FROM 中生效
  ARG DOCKER_USERNAME=library
  
  FROM ${DOCKER_USERNAME}/alpine
  
  # 要想在 FROM 之后使用，必须再次指定
  ARG DOCKER_USERNAME=library
  
  RUN set -x ; echo ${DOCKER_USERNAME}
  ```

  对于多阶段构建，FROM 中都生效。

  
  
  ```shell
  # 这个变量在每个 FROM 中都生效
  ARG DOCKER_USERNAME=library
  
  FROM ${DOCKER_USERNAME}/alpine
  
  RUN set -x ; echo 1
  
  FROM ${DOCKER_USERNAME}/alpine
  
  RUN set -x ; echo 2
  ```
  

* 在FROM 后使用，需要各个阶段分别指定

  ```shell
  ARG DOCKER_USERNAME=library
  
  FROM ${DOCKER_USERNAME}/alpine
  
  # 在FROM 之后使用变量，必须在每个阶段分别指定
  ARG DOCKER_USERNAME=library
  
  RUN set -x ; echo ${DOCKER_USERNAME}
  
  FROM ${DOCKER_USERNAME}/alpine
  
  # 在FROM 之后使用变量，必须在每个阶段分别指定
  ARG DOCKER_USERNAME=library
  
  RUN set -x ; echo ${DOCKER_USERNAME}
  ```

#### VOLUME 定义卷名

格式为：

* VOLUME ["<路径1>","<路径2>"...]
* VOLUME <路径>

```shell
VOLUME /data
```

不会记录到容器存储层

挂在：

```shell
docker run -d -v mydata:/data XXX
```

代替了Dockerfile 中定义的匿名卷的挂载位置。

#### EXPOSE 暴露端口

格式为：EXPOSE <端口1> [<端口2>...]

只是申明，决定端口还是运行时 -p <宿主端口>:<容器端口>。

#### WORKDIR 指定工作目录

格式：WORKDIR <工作目录路径>

错误：

```shell
RUN cd /app
RUN echo "hello" > world.txt	
```

这样会创建两层，两层报错，找不到指定文件报错

正确：

```shell
WORKDIR /app

RUN echo "hello" > world.txt
```

WORKDIR 相对路径设置：

```shell
WORKDIR /a
WORKDIR b
WORKDIR c

RUN pwd # /a/b/c
```

#### USER 指定当前用户

格式：USER <用户名>:[:<用户组>]

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425165251485.png" alt="image-20220425165251485" style="zoom:50%;" />

#### HEALTHCHECK 健康检查

```shell
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
  CMD curl -fs http://localhost/ || exit 1
```

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425170642195.png" alt="image-20220425170642195" style="zoom:50%;" />

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425170710330.png" alt="image-20220425170710330" style="zoom:50%;" />

#### ONBUILD 为他人作嫁衣

#### LABEL 为镜像添加元数据

```shell
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

标签申明作者、文档地址：

```shell
LABEL org.opencontainers.image.authors="yeasy"

LABEL org.opencontainers.image.documentation="https://yeasy.gitbooks.io"
```

#### SHELL 指令

格式：SHELL ["executable","parameters"]

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425174627212.png" alt="image-20220425174627212" style="zoom:50%;" />

#### Dockerfile 多阶段构建

* **全部放入一个 Dockerfile 中，包括项目及其依赖库的编译、测试、打包等流程**

  问题：

  * 镜像层次多，镜像体积较大，部署时间变长
  * 源代码存在泄露风险

* **分散到多个 Dockerfile**

		1. 在一个 Dockerfile 中将项目依赖库编译测试打包好
		1. 将其拷贝到运行环境中。需要编写两个 Dockerfile 和一些编译脚本，将两个阶段自动整合起来。

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220425175303627.png" alt="image-20220425175303627" style="zoom:50%;" />



















