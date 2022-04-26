---
title: DevOps 漫谈：Docker
date: 2017-08-21 13:46:54
tags: [DevOps,架构师]
categories: 工程技术
---
## 摘要
- Docker ABC:常用命令、自定义制作、发布

<!--more-->

This article is part of an **Virtualization Technology** tutorial series. Make sure to check out my other articles as well:

- [2018 年度 Docker 用户报告 - Sysdig Edition ](https://riboseyim.github.io/2018/06/12/DevOps-Container-Usage/)
- [Cyber-Security: Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [DevOps漫谈：Docker ABC](https://riboseyim.github.io/2017/08/21/DevOps-Docker/)

![](https://cdn-images-1.medium.com/max/1600/1*easlVE_DOqRDUDkVINRI9g.png)

## ABC
- Docker 常用命令
- Docker 自定义制作

#### Docker 常用命令

```bash
$ docker version
$ docker info

# service 命令的用法
$ sudo service docker start

# 列出本机的所有 image 文件。
$ docker image ls
# 删除 image 文件
$ docker image rm [imageName]

# Hello World
$ docker image pull hello-world
$ docker container run hello-world

# 启动已经生成、已经停止运行
$ docker container start [containerID]
# 终止容器 [SIGTERM]
$ docker container stop [containerID]
# 终止容器 [SIGKILL]
$ docker container kill [containID]
# 列出本机正在运行的容器
$ docker container ls

# 列出本机所有容器，包括终止运行的容器
$ docker container ls --all

# 进入一个正在运行的容器
$ docker container exec -it [containerID] /bin/bash
# 将文件从容器拷贝到本机
$ docker container cp [containID]:[/path/to/file] .
$ docker container logs

```

#### Docker 自定义制作

- 项目根目录创建 .dockerignore 文件(需要忽略打包的文件)
```
.git
logs
```
- 创建 Dockerfile 文件

- 构建 image 文件
```bash
## You need to add a dot（"docker build" requires exactly 1 argument(s)）
$ docker image build -t gospider .
Sending build context to Docker daemon  12.47MB
Step 1/4 : FROM node:8.4
8.4: Pulling from library/node
aa18ad1a0d33: Downloading [===============>                                   ]  16.18MB/52.6MB
90f6d19ae388: Download complete
94273a890192: Downloading [================>                                  ]  14.13MB/43.23MB
c9110c904324: Downloading [===>                                               ]  10.25MB/134.7MB
788d73c0fb6b: Waiting
f221bb562f24: Waiting
14414a6a768d: Waiting
af6d2b2ad991: Waiting
......
14414a6a768d: Pull complete
af6d2b2ad991: Pull complete
Digest: sha256:080488acfe59bae32331ce28373b752f3f848be8b76c2c2d8fdce28205336504
Status: Downloaded newer image for node:8.4
 ---> 386940f92d24
Step 2/4 : COPY . /app
 ---> 5dea8aef5c00
Step 3/4 : WORKDIR /app
Removing intermediate container de21dc94fd49
 ---> c04960e1036b
Step 4/4 : EXPOSE 3001
 ---> Running in 7e5efe5e0b43
Removing intermediate container 7e5efe5e0b43
 ---> bcadd82be353
Successfully built bcadd82be353
Successfully tagged gospider:latest

$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gospider            latest              bcadd82be353        7 minutes ago       685MB
```

- 制作容器
docker container run 从 image 文件生成容器。
可选参数：
- -p  参数：p1:p2 [容器端口]映射到[本机端口]
- imagename
- /bin/bash：保证用户可以使用 Shell

```bash
$ docker container run -it gospider /bin/bash
root@d2d7bfd9f4c1:/app#
root@d2d7bfd9f4c1:/app#
root@d2d7bfd9f4c1:/app# ls
Dockerfile		     
gmof_caipiao_month.go	    gmof_cdc_epide_month.go	    publish		    ttank_zyuc_handle.go
charset.go		     gmof_caipiao_month_spider.go   gmof_cdc_epide_month_spider.go  test		    ttank_zyuc_register.go
dbutils_csv.go		     gmof_casad_info_spider.go	    gmof_person.go		    ttank-echarts	    zyuc_task_info.go
dbutils_kafka.go	     gmof_casad_list.go		    gmof_person_region.go	    ttank.go		    zyuc_task_spider.go
dbutils_pg.go		     gmof_casad_list_spider.go	    gmof_person_region_spider.go    ttank_gmof_handle.go    
gmof_caipiao_list.go	     gmof_cdc_epide_list.go	    kafka			    ttank_gmof_register.go
gmof_caipiao_list_spider.go  gmof_cdc_epide_list_spider.go  ops				    ttank_tasks.go
root@d2d7bfd9f4c1:/app#
root@d2d7bfd9f4c1:/app# id
uid=0(root) gid=0(root) groups=0(root)
root@d2d7bfd9f4c1:/app# whoami
root
```

#### Docker 发布
- 注册账户：[hub.docker.com](https://hub.docker.com) 或 [cloud.docker.com](https://cloud.docker.com)
- 登录
```bash
$ docker login
Login with your Docker ID to push and pull images from Docker Hub.
If you don\'t have a Docker ID, head over to https://hub.docker.com to create one.
Username: riboseyim
Password:
Login Succeeded

# 为本地的 image 标注用户名和版本。
$ docker image tag [imageName] [username]/[repository]:[tag]

# 发布
$ docker image push riboseyim/gospider
The push refers to repository [docker.io/riboseyim/gospider]
db602e58edca: Pushed
c5b2ac536264: Mounted from library/node
6c08a5b7d8f4: Mounted from library/node
50867bb8f5d7: Mounted from library/node
72d6c6f0ea06: Mounted from library/node
8686c6b8d999: Mounted from library/node
44b57351135e: Mounted from library/node
00b029f9aa09: Mounted from library/node
18f9b4e2e1bc: Mounted from library/node
latest: digest: sha256:4388ba2cc83adc0d1e9b3ca4c0b273f5812ad6365a36aeef33ff87d62b236d5d size: 2217
```

#### Docker 构建

- [A tool for building/building artifacts from source and injecting into docker images](https://github.com/openshift/source-to-image)

## Examples

- [The Ultimate Guide to Writing Dockerfiles for Go Web-apps](https://blog.hasura.io/the-ultimate-guide-to-writing-dockerfiles-for-go-web-apps-336efad7012c)

## Docker 安全
- [Cyber-Security:Linux容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
1.容器操作系统与多租户
2.容器内容（使用可信源）
3.容器注册 (容器镜像加密访问)
4.构建过程安全
5.控制集群中可部署的内容
6.容器编排：加强容器平台安全
7.网络隔离
8.存储
9.API 管理, 终端安全和单点登录 (SSO)
10.角色和访问控制管理

## Docker 网络
- [docker 容器网络方案：calico 网络模型 | Cizixs](http://cizixs.com/2017/10/19/docker-calico-network)

## Docker 监控

- [搭建Docker监控框架的理论与范例 | 2016-07-22 穆寰 InfoQ](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650993402&idx=2&sn=39583a788d89d62ecb4953d4a9f8635a&scene=1&srcid=0722P3T1gARgmDlPwn8ZDNr9%23rd)

## Docker 参考书

- [Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)
- [Docker — 从入门到实践](https://www.yuque.com/grasilife/docker)


## 扩展阅读

#### [DevOps 漫谈系列](https://riboseyim.github.io/2016/07/28/DevOps/)
- [《凤凰项目》：从作坊到工厂的寓言故事](https://riboseyim.github.io/2018/04/10/DevOps-Phoenix/)
- [Kanban 看板管理实践](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
- [DevOps 漫谈：基础设施部署和配置管理](https://riboseyim.github.io/2018/03/26/DevOps-Deployment/)
- [Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)

#### [DevOps 实践的本质是文化](https://riboseyim.github.io/2018/03/29/DevOps-Culture/)
- 学习力－团队生命之根
- 带领团队翻译书籍
- Don’t make me think
- 凡是被很多人不断重复的好习惯，要将其自动化整合到工具

## 参考文献
- [Quick Guide on Docker Utilities, Daemon and its other capabilities](https://www.linuxtechi.com/guide-docker-utilities-daemon-capabilities/)
- [How to Remove Docker Images, Networks, Containers and Volumes](https://linux4one.com/how-to-remove-docker-images-networks-containers-and-volumes/)
- [A fast and easy Docker tutorial for beginners (video series)](https://medium.freecodecamp.org/docker-quick-start-video-tutorials-1dfc575522a0)
- [Docker 微服务教程](http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
- [在 Mac 上跳舞的容器 — Docker |  2016-09-06 池建强 MacTalk](https://mp.weixin.qq.com/s?__biz=MjM5ODQ2MDIyMA==&mid=2650712620&idx=1&sn=39b33e0f1dfc335e165051b2983f9192&scene=1&srcid=0906ZUDNyflKkccX0TptpfJM%23rd)
- [AgentNEO 架构简介 | 07 May 2018](https://agentneoteam.github.io/2018/05/07/agentneo-jia-gou-jian-jie.html)
- [A Practical Introduction to Container Terminology](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/)
- [Why Docker is Not Yet Succeeding Widely in Production,2015 | Simon Hørup Eskildsen ](http://sirupsen.com/production-docker/)
- [Dockercon Talk | Resilient Routing and Discovery | Simon Hørup Eskildsen ](https://www.youtube.com/watch?v=ZDeAEZHby_A)
- [数据库不适合Docker及容器化的7大原因 | 2017-02-14 Mikhail Chinkov 高可用架构](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=2653548247&idx=1&sn=99d0e90fa99deec3a7dab49eac418e5c&chksm=813a7f4fb64df65946223f717a5231aae62edc5b6335613e45f8153367736ed480e9439d8906&mpshare=1&scene=1&srcid=0215WY6P1gGeSGjrVAD75iaB%23rd)
- [Docker, Inc is Dead | Posted on December 30, 2017  (Last modified on February 9, 2018)](https://chrisshort.net/docker-inc-is-dead/)
- [关于Docker实际采用情况的八个惊人真相 | 2016-06-22 云头条](https://mp.weixin.qq.com/s?__biz=MjM5MzM3NjM4MA==&mid=2654676719&idx=2&sn=5d3e73befafe752d6e84788dcb6b33ec&scene=1&srcid=0622QOd24IK0rdOypNUq1BcA%23rd)
- [Open-sourcing gVisor, a sandboxed container runtime | Wednesday, May 2, 2018](https://cloudplatform.googleblog.com/2018/05/Open-sourcing-gVisor-a-sandboxed-container-runtime.html)
- [A Practical Introduction to Container Terminology](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/)
