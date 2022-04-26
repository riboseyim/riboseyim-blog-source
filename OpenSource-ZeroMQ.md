---
title: 开源技术架构漫谈：消息中间件(ZeroMQ)
date: 2016-04-26 11:56:32
tags: [OpenSource]
categories: 工程技术
---
## 摘要
>ZeroMQ (also known as ØMQ, 0MQ, or zmq) is an networking library, acts like a concurrency framework。https://github.com/zeromq-cn

>缘起

first time I knew:[Ntopng](http://www.jianshu.com/p/f10a2d862863)

<!--more-->

#### 发起人：Pieter Hintjens
iMatix公司首席执行官，制作游戏视频游戏起家。Wikidot公司前CEO。数字标准组织创始人。曾任FFII（自由信息基础设施基金会）会长，与软件专利斗争的大型NGO组织。会说英语、法语、荷兰语。生活在比利时布鲁塞尔，组织西非鼓乐队，喜欢旅行。一个C内核及C＋＋编写的核心库libzmq，50余种语言支持的绑定程序(例如Python支持PyZMQ)。由绑定程序的作者组成维护小组，iMatix公司持有商标，提供商业支持。

## 架构哲学
零代理，零延迟，零管理，零成本，也代表贯穿项目的简约文化。
软件的真正本质是人的本质，我们人类真的不善于理解复杂性，并且我们真的善于用合作来对大问题分而治之。我们是高度社会化的类人猿，有点聪明，但只在合适的人群中。

请把架构视为一个市场，而不是一个工程设计。－－《来自ZeroMQ的软件架构哲学》


#### Repository
- http://zeromq.org/   

#### Application
![](http://riboseyim-qiniu.riboseyim.com/theme_opensource_zeromq.png)

- Ntopng:Packet Capturing and Network Traffic Monitor
- [SaltStack](https://riboseyim.github.io/2018/09/18/DevOps-Deployment-SaltStack):基础设施管理工具（Based Python）
- Apache SINGA:分布式深度学习平台
- 微软DMTK:机器学习框架，ZMQ是进程通讯之一（2015-11-16 开源）
- IPython Notebook
  既是一个交互计算平台，又是一个记录计算过程的「笔记本」。它由服务端和客户端两部分组成，其中服务端负责代码的解释与计算，而客户端负责与用户进行交互。服务端可以运行在本机也可以运行在远程服务器，包含负责运算的 IPython kernel (与 QT Console 的 kernel 相同) 以及一个 HTTP/S 服务器 (Tornado)。而客户端则是一个指向服务端地址的浏览器页面，负责接受用户的输入并负责渲染输出。http://ipython.org/notebook.html


#### ABC

Hello ZeroMQ

#### Install

```bash
git clone --depth=1 https://github.com/imatix/zguide.git
./configure
make
make install
```

#### 编译依赖项
**libsodium-1.0.0** (zeromq-4.1.2) 

Github Issues:[zmq-4.1.2 make failed as libsodium-1.0.8 sodium_init #1854](https://github.com/zeromq/libzmq/issues/1854)

```bash
cc1plus: warnings being treated as errors
src/curve_client.cpp: In constructor

'zmq::curve_client_t::curve_client_t(const zmq::options_t&)':
src/curve_client.cpp:61: warning: ignoring return value of
 'int sodium_init()',
declared with attribute warn_unused_result
```

```bash
//C Project 添加库
gcc -L/usr/local/lib -o "veto_mq_server"  ./src/veto_mq_server.o  -lzmq
```

**可能的异常：库路径、名称错误**

```bash
Undefined symbols for architecture x86_64:
"_zmq_bind", referenced from:
_main in veto_mq_server.o
"_zmq_ctx_new", referenced from:
_main in veto_mq_server.o
"_zmq_recv", referenced from:
_main in veto_mq_server.o
"_zmq_send", referenced from:
_main in veto_mq_server.o
"_zmq_socket", referenced from:
_main in veto_mq_server.o
ld: symbol(s) not found for architecture x86_64  
clang: error: linker command failed with exit code 1 (use -v to see invocation)**
```

#### Client-Server Model

```c
//server.c
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <assert.h>
#include <zmq.h>
#include <zmq_utils.h>

#include "veto_mq_utils.h"

int main (void)
{
 //
 print_zmq_version();
    //  Socket to talk to clients
    void *context = zmq_ctx_new ();
    void *responder = zmq_socket (context, ZMQ_REP);
    int rc = zmq_bind (responder, "tcp://*:5555");
    assert (rc == 0);

    while (1) {
        char buffer [10];
        zmq_recv (responder, buffer, 10, 0);

        printf ("Received Hello \n");
        sleep (1);          //  Do some 'work'
        zmq_send (responder, "World", 5, 0);
    }
    return 0;
}
```

```c
//client.c

#include <string.h>
#include <stdio.h>
#include <unistd.h>

#include <zmq.h>
#include <zmq_utils.h>

int main(void) {

   printf ("Connecting to hello world server...\n");

     void \*context = zmq_ctx_new ();
     void \*requester = zmq_socket (context, ZMQ_REQ);

     zmq_connect (requester, "tcp://localhost:5555");

     int request_nbr;
     for (request_nbr = 0; request_nbr != 10; request_nbr++) {
         char buffer [10];
         printf ("Sending Hello %d...\n", request_nbr);
         zmq_send (requester, "Hello", 5, 0);
         zmq_recv (requester, buffer, 10, 0);
         printf ("Received World %d\n", request_nbr);
     }

     zmq_close (requester);
     zmq_ctx_destroy (context);

  return 0;
}
```


```bash
Makefile
SHELL   = /bin/sh
prefix  = /usr/local
exec_prefix=${prefix}
srcdir  = .
sbindir = ${exec_prefix}/sbin
libdir  = ${exec_prefix}/lib
sysconfdir = ${prefix}/etc
SETUPDIR = /home/nms
TARGETDIR = $(SETUPDIR)/bin
SRCDIR = .
CC      = gcc
CFLAGS  = -O2 -Wall
CPPFLAGS= -g 
DEFS    = 
LDFLAGS = 
LIBS    = -lzmq  -L/usr/local/lib
INCLUDES= -I/usr/local/include 

CURR_DIR = $(shell pwd)

CC_TMP = lib

OBJS    =  $(CC_TMP)/veto_mq_server.o  $(CC_TMP)/veto_mq_utils.o
TARGET  =  veto_mq_server

.c.o:
 $(CC) -I. $(CPPFLAGS) $(DEFS) $(CFLAGS) -c $<

all: $(TARGET)

$(TARGET): $(OBJS)
 $(CC) $(CPPFLAGS) $(OBJS) $(LIBS) $(LDFLAGS) -o $@
 -cp $(SRCDIR)/$(TARGET)  $(TARGETDIR)/$(TARGET) 
 -chmod ugo+s $(TARGETDIR)/$(TARGET) 
 -chmod ugo+s $(SRCDIR)/$(TARGET) 

clean:
 -rm -f *.o *~  *.core core $(TARGET) 
 -rm -f $(TARGETDIR)/$(TARGET)

veto_mq_server.o: veto_mq_server.c
 $(CC) -g -c -o $(CC_TMP)/veto_mq_server.o veto_mq_server.c $(CPPFLAGS) $(INCLUDES)

veto_mq_utils.o: veto_mq_utils.c
 $(CC) -g -c -o $(CC_TMP)/veto_mq_utils.o veto_mq_utils.c $(INCLUDES)

```

#### Running

```bash
bash-3.2# ./veto_mq_server

bash-3.2# ps -ef | grep mq | grep -v 'grep'
    0  3616     1   0 10:00AM ttys000    0:00.01 ./veto_mq_server

```

```bash
bash-3.2# ./veto_mq_client
Connecting to hello world server...
Sending Hello 0...
Received World 0
Sending Hello 1...
Received World 1
Sending Hello 2...
Received World 2
Sending Hello 3...
Received World 3
```

## 参考文献
- （推荐）［美］Pieter Hintjens《ZeroMQ:Messaging for Many Applications》
- [ZeroMQ社区生态白皮书](http://www.jianshu.com/p/8ddb7e19ce46)
- [深入解析中间件之RocketMQ](http://github.com/zqhxuyuan/2017/10/18/Midd-RocketMQ/)
