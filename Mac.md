---
title: Mac OS 操作系统专题
date: 2019-01-05 10:51:13
tags: [工具癖,DevOps,Mac,OperatingSystem]
categories: 工程技术
---
## 摘要

- The History
- Overview of the Mac
- Application and Development
- Future

<!--more-->

This article is part of an **Mac Operating System** tutorial series. Make sure to check out my other articles as well:

- [谁是王者：macOS vs Linux Kernels ？](https://riboseyim.com/2017/12/19/Linux-Win-Mac/)
- [数据可视化（一）思维利器 OmniGraffle for Mac 使用指南](https://riboseyim.com/2017/09/15/Visualization-OmniGraffle/)
- [我的写作工具链](https://riboseyim.com/2017/06/03/Writing-WriterToolChain/)

## The Big Picture

### The History

- [谁是王者：macOS vs Linux Kernels ？](https://riboseyim.com/2017/12/19/Linux-Win-Mac/)

## Overview of the Mac

#### 快键键

```
⌘——Command ()

⌃ ——Control

⌥——Option (alt)

⇧——Shift

⇪——Caps Lock

fn——功能键
```

#### Basic Commands

| 操作         | Linux                             | Mac                                         | Windows             |
| ------------ | --------------------------------- | ------------------------------------------- | ------------------- |
| 字符匹配     | grep                              | grep                                        | findstr             |
| 查看网卡     | ifconfig                          | ifconfig                                    | ipconfig            |
| 查看网络路由 | route -n <br> netstat -ar         | netstat -ar                                 | route PRINT         |
| 查看网络连接 | netstat -an                       | netstat -an                                 | netstat -an         |
| 更新动态库   | ldconfig                          | update_dyld_shared_cache                    | ----                |
| hosts        | /etc/hosts/<br>/private/etc/hosts | -----                                       |                     |
| debug        | strace                            | dtruss                                      | -----               |
| 查看库依赖   | ldd                               | otool -L                                    | ----                |
| 进程管理     | ps                                | ps                                          | tasklist            |
| 进程管理     | kill                              | kill                                        | taskkill /pid /T /F |
| 环境变量     | env                               | [print]printenv <br> [init] ~/.bash_profile |                     |
| 压缩加密             |                                   |       zip -e ./jiami.zip aa.txt                                      |                     |
|              |                                   |                                             |                     |

#### 磁盘管理

- diskutil

```
$ diskutil list
$ diskutil mountDisk /dev/disk2
Volume(s) mounted successfully
$ diskutil unmountDisk /dev/disk2
Unmount of all volumes on disk2 was successful
```

#### 网络管理

- NAT

```
/Library/Preferences/VMware Fusion/vmnet8/nat.conf
# NAT gateway address
ip = 192.168.213.2
netmask = 255.255.255.0
```

## Application and Development

### Personal Level

- [我的写作工具链](https://riboseyim.com/2017/06/03/Writing-WriterToolChain/)

### Enterprise Level

- [数据可视化（一）思维利器 OmniGraffle for Mac 使用指南](https://riboseyim.com/2017/09/15/Visualization-OmniGraffle/)

#### Hardware
- [How to Fix Early 2015 MacBook Pro Touchpad Keyboard](https://www.youtube.com/watch?v=FzcBL1NBtik)
- [Keyboard and trackpad don't work, loose cable?](https://zh.ifixit.com/Answers/View/313001/Keyboard+and+trackpad+don%27t+work,+loose+cable)
- [touchpad TrackPad 丝带排线替换821 – 00184-a 适用于 Apple MacBook Pro Retina 33 cm A1502早2015](https://www.amazon.com/gp/product/B0711CZPXJ/ref=oh_aui_detailpage_o00_s00?ie=UTF8&psc=1&tag=ifixitam-20)


## 扩展阅读

## 参考文献
- [What’s New in Apple Filesystems](https://devstreaming-cdn.apple.com/videos/wwdc/2019/710aunvynji5emrl/710/710_whats_new_in_apple_file_systems.pdf)
- [推荐一些 Mac 上比较好用的软件](https://mp.weixin.qq.com/s/Q2IEE4t3naR6j3iP78V29g)
- [scomper:OmniFocus 2 for Mac 的使用指南](http://www.jianshu.com/p/9257e1e7ac39)
- [OmniPlan 使用教程 | OmniPlan 3 for Mac 用户手册](https://support.omnigroup.com/doc-assets/OmniPlan-Mac/OmniPlan-Mac-v3.0.0.1/zh/EPUB/xhtml/03_tutorial.xhtml)
