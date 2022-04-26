---
title: 玩转编程语言:编译原理
date: 2017-05-12 19:03:30
tags: [Developer]
categories: 工程技术
---
## 摘要
- LLVM（Low Level Virtual Machine）
- 构建 LLVM 和 Clang 开发工具库

<!--more-->

#### A Go compiler and runtime

The language spec is a text document, which is not useful in and of itself. For that you need software that actually implements these semantics. This is done by a compiler (which analyzes and checks the source code and transforms it into an executable format) and a runtime (which provides the necessary environment to actually run the code). There are many such combinations and they all differ a bit more or a bit less. Examples are

- gc, a compiler and runtime written in pure Go (with some assembly) by the Go team themselves and versioned and released together with the language. Unlike other such tools, gc doesn't strictly separate the compiler, assembler and linker - they end up sharing a lot of code and some of the classical responsibilities move or are shared between them. As such, it's in general not possible to e.g. link packages compiled by different versions of gc.

- gccgo and libgo, a frontend for gcc and a runtime. It's written in C and maintained by the Go team. It lives in the gcc organization though and is released according to the gcc release schedule and thus often lags a bit behind the "latest" version of the Go spec.

- llgo, a frontend for LLVM. I don't know much else about it.
gopherjs, compiling Go code into javascript and using a javascript VM plus some custom code as a runtime. Long-term, it'll probably be made obsolete by gc gaining native support for WebAssembly.

- tinygo, an incomplete implementation targeting small code size. Runs on either bare-metal micro-controllers or WebAssembly VMs, with a custom runtime. Due to its limitations it doesn't technically implement Go - notably, it doesn't include a garbage collector, concurrency or reflection.

## LLVM

LLVM（Low Level Virtual Machine）是一种编译器基础设施，以 C++ 写成。起源于 2000 年伊利诺伊大学厄巴纳-香槟分校维克拉姆·艾夫（Vikram Adve）与克里斯·拉特纳（Chris Lattner）的研究，他们想要为所有静态及动态语言创造出动态的编译技术。2005 年，Apple 直接雇用了克里斯·拉特纳及他的团队，为了苹果电脑开发应用程序，期间克里斯·拉特纳设计发明了 Swift 语言，LLVM 成为 Mac OS X 及 iOS 开发工具的一部分。LLVM 的范围早已经不局限于创建一个虚拟机，成为了众多编译工具及低级工具技术的统称，适用于 LLVM 下的所有项目，包含 LLVM 中介码（LLVM IR）、LLVM 除错工具、LLVM C++ 标准库等。

目前 LLVM 已支持包括 ActionScript、Ada、D 语言、Fortran、GLSL、Haskell、Java字节码、Objective-C、Swift、Python、Ruby、Rust、Scala1 以及 C# 等语言。

#### LLVM 与 Clang

Clang 是LLVM编译器工具集的前端（front-end），目的是输出代码对应的抽象语法树（Abstract Syntax Tree, AST），并将代码编译成LLVM Bitcode。接着在后端（back-end）使用 LLVM 编译成平台相关的机器语言 。Clang支持C、C++、Objective C。它的目标是提供一个 GCC 的替代品。作者是克里斯·拉特纳（Chris Lattner），在苹果公司的赞助支持下进行开发。Clang项目包括Clang前端和Clang静态分析器等。

#### 构建 LLVM 和 Clang 开发工具库

```bash
# MacOS
$ brew install --with-toolchain llvm
$ brew info llvm
# Linux
$ yum install gcc
$ yum install gcc-g++

wget https://cmake.org/files/v3.9/cmake-3.9.0-rc4.tar.gz
tar -xvf cmake-3.9.0-rc4.tar.gz
cd cmake-3.9.0
./bootstrap  
gmake
gmake install
export CMAKE_ROOT=/usr/local/share/cmake-3.9.0
export PATH=$PATH:$CMAKE_ROOT

git clone http://llvm.org/git/llvm.git
cd llvm/tools;
git clone http://llvm.org/git/clang.git
cd ..; mkdir -p build/install; cd build
cmake -G "Unix Makefiles" -DLLVM_TARGETS_TO_BUILD="BPF;X86" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$PWD/install ..
make
make install
export PATH=$PWD/install/bin:$PATH
```

## Tips

#### glibc

```sh
$ wget  http://ftp.gnu.org/pub/gnu/glibc/glibc-2.17.tar.gz
$ tar -zxvf glibc-2.17.tar.gz
$ cd glibc-2.17
$ mkdir build
$ cd build
$ ../configure
    --prefix=/usr
    --disable-profile
    --enable-add-ons
    --with-headers=/usr/include
    --with-binutils=/usr/bin
$ make && make install
# 验证
$ strings /lib64/libc.so.6|grep GLIBC

```

#### glibc++
```sh
$ yum provides libstdc++.so.6

# 安装完成之后查看
$ strings /usr/local/lib64/libstdc++.so.6.0.20 | grep GLIBCXX_
# 发现有了相应的3.4.20支持
$ ln -sf /usr/local/lib64/libstdc++.so.6.0.20 /usr/lib64/libstdc++.so.6
```
## 扩展阅读

- [LLVM IR and Go](https://blog.gopheracademy.com/advent-2018/llvm-ir-and-go/)
![](https://blog.gopheracademy.com/postimages/advent-2018/llvm-ir-and-go/llvm_compiler_pipeline.png)

#### [玩转编程语言系列](https://riboseyim.github.io/2017/05/26/Language/)


## 参考文献
- [A bird's eye view of Go](https://blog.merovius.de/2019/06/12/birdseye-go.html)
- [LLVM for Grad Students](http://www.cs.cornell.edu/~asampson/blog/llvm.html)
- [Gentle introduction into compilers. Part 1: Lexical analysis and Scanner | A guide to understanding ECMAScript (JavaScript) spec lexical grammar and TypeScript scanner](https://medium.com/dailyjs/gentle-introduction-into-compilers-part-1-lexical-analysis-and-scanner-733246be6738)
