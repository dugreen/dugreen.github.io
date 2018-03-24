---
layout:     post
title:      "Dockeri在Ubuntu平台的安装与使用"
subtitle:   ""
date:       2018-3-10 12:00:00
author:     "Dugreen"
header-img: "img/artical-bgs/back7.jpg"
catalog: true
tags:
     - linux
---

## Docker介绍

> Docker是一个可以使开发者简单方便地在一个容器(自行配置或者pull第三方)中运行应用的应用软件。Docker和我们平时用的虚拟机很相似，但是Docker更加的便携，资源友好，更加依赖主机操作系统。

更多关于Docker的介绍可以访问这个[页面](https://www.digitalocean.com/community/tutorials/the-docker-ecosystem-an-introduction-to-common-components)

## 安装之前的要求

* 64-bit Ubuntu
* Non-root user with sudo privileges

## 安装Docker

首先，添加官方Docker仓库的GPG key到系统中。

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

添加Docker仓库到APT source中。

```
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

更新软件包数据库。

```
$ sudo apt-get update
```

确定你即将从Docker官方仓库下载安装Docker而不是ubuntu的仓库中。

```
$ apt-cache policy docker-ce
```

你应该看到下面的输出：

```
docker-ce:
  Installed: (none)
  Candidate: 17.03.1~ce-0~ubuntu-xenial
  Version table:
     17.03.1~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     17.03.0~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

安装Docker

```
$ sudo apt-get install -y docker-ce
```

现在Docker应该已经被安装了，daemon已经开启，让我们检查一下Docker是否成功运行。

```
$ sudo systemctl status docker
```

你将会看到下面的输出：

```
Output
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2016-05-01 06:53:52 CDT; 1 weeks 3 days ago
     Docs: https://docs.docker.com
 Main PID: 749 (docker)
```

OK,安装成功。

## 执行Docker命令不需Sudo(可选)

在shell中直接输入Docker的命令可能会产生下面的输出：

```
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
```

如果你想避免在执行Docker命令的时候输入sudo,你可以添加你的用户名到docker group中。

```
$ sudo usermod -aG docker ${USER}
```

## 开始接触Docker命令

Docker命令的使用语法形式：

```
$ docker [option] [command] [arguments]
```

查看全部的可使用命令：

```
$ docker
```

输出如下：

```
Output

    attach    Attach to a running container
    build     Build an image from a Dockerfile
    commit    Create a new image from a container's changes
    cp        Copy files/folders between a container and the local filesystem
    create    Create a new container
    diff      Inspect changes on a container's filesystem
    events    Get real time events from the server
    exec      Run a command in a running container
    export    Export a container's filesystem as a tar archive
    history   Show the history of an image
    images    List images
    import    Import the contents from a tarball to create a filesystem image
    info      Display system-wide information
    inspect   Return low-level information on a container or image
    kill      Kill a running container
    load      Load an image from a tar archive or STDIN
    login     Log in to a Docker registry
    logout    Log out from a Docker registry
    logs      Fetch the logs of a container
    network   Manage Docker networks
    pause     Pause all processes within a container
    port      List port mappings or a specific mapping for the CONTAINER
    ps        List containers
    pull      Pull an image or a repository from a registry
    push      Push an image or a repository to a registry
    rename    Rename a container
    restart   Restart a container
    rm        Remove one or more containers
    rmi       Remove one or more images
    run       Run a command in a new container
    save      Save one or more images to a tar archive
    search    Search the Docker Hub for images
    start     Start one or more stopped containers
    stats     Display a live stream of container(s) resource usage statistics
    stop      Stop a running container
    tag       Tag an image into a repository
    top       Display the running processes of a container
    unpause   Unpause all processes within a container
    update    Update configuration of one or more containers
    version   Show the Docker version information
    volume    Manage Docker volumes
    wait      Block until a container stops, then print its exit code
```

查看关于Docker的系统信息：

```
$ docker info
```

## Docker镜像操作

> Docker容器是从Docker镜像运行的。大家有使用过github的可能会知道，各种不同的代码项目保存在Hub中，Docker也有类似的Hub(Docker hub).这个Hub存储了每个用户和官方的镜像。每个用户可以pull你想获取的镜像。

首先检查你是否可以访问Docker Hub并下载镜像。(Docker 的 HelloWorld)

```
$ docker run hello-world
```

你应该可以看到如下的输出：

```
Output
Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```

你可以在DockerHub上搜索你需要的镜像：

```
$ docker search ubuntu
```

你应该可以看到这样的输出：

```
Output

NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                            Ubuntu is a Debian-based Linux operating s...   3808      [OK]       
ubuntu-upstart                    Upstart is an event-based replacement for ...   61        [OK]       
torusware/speedus-ubuntu          Always updated official Ubuntu docker imag...   25                   [OK]
rastasheep/ubuntu-sshd            Dockerized SSH service, built on top of of...   24                   [OK]
ubuntu-debootstrap                debootstrap --variant=minbase --components...   23        [OK]       
nickistre/ubuntu-lamp             LAMP server on Ubuntu                           6                    [OK]
nickistre/ubuntu-lamp-wordpress   LAMP on Ubuntu with wp-cli installed            5                    [OK]
nuagebec/ubuntu                   Simple always updated Ubuntu docker images...   4                    [OK]
nimmis/ubuntu                     This is a docker images different LTS vers...   4                    [OK]
maxexcloo/ubuntu                  Docker base image built on Ubuntu with Sup...   2                    [OK]
admiringworm/ubuntu               Base ubuntu images based on the official u...   1                    [OK]

...

```

搜索到自己想要的镜像后，可以pull到本地：

```
$ docker pull ubuntu
```

在pull到本地后就可以运行这个镜像了(这里的运行是查看镜像是否可用)：

```
$ docker run ubuntu
```

再次查看本地已经存在的镜像库：

```
$ docker images
```

## 开始运行一个Docker容器

现在系统本地库中已经有了一个ubuntu和hello-world,接下来我们运行一个容器来跑ubuntu

```
$ docker run -it ubuntu
```

你接下来会看到下面的输出：

```
Outpul:  # 请忽略这句话
root@d9b100f2f636:/#

```

到这里不用我再介绍什么了，我们已经进入了一个全新的独立的系统。

当我们想退出这个容器的时候我们必须要commmit才可以保存刚才的对系统的修改。

```
$ exit
$ docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name
```

然后查看本地库镜像：

```
$ docker images
```

现在你会看到刚才你commit的那个镜像:

```
Output
finid/ubuntu-nodejs       latest              62359544c9ba        50 seconds ago      206.6 MB
ubuntu              latest              c5f1cf30c96b        7 days ago          120.8 MB
hello-world         latest              94df4f0ce8a4        2 weeks ago         967 B
```

## 参考

[digital-ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)

