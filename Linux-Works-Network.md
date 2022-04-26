---
title: How Linux Works:Network
date: 2018-01-08 04:28:31
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要
- 一、How DNS Works
- 二、How Route Works
- 三、How ARP Works

<!--more-->

## How DNS Works

![Overview](http://riboseyim-qiniu.riboseyim.com/Linux-DNS-Overview.png)

域名系统（Domain Name System，DNS）将域名和 IP 地址相互映射，能够使人更方便地访问互联网。DNS 最早于 1983 年由保罗·莫卡派乔斯（Paul Mockapetris）发明（RFC 882，RFC 883），1987年发布了修正（RFC 1034，RFC 1035），在此之后 DNS 技术基本上没有改动。

DNS 协议使用端口 53 ，同时兼容 TCP (RFC-793) 和 UDP(RFC-768) ，但是考虑到更低的开销及性能，DNS 查询通常使用 UDP 协议。DNS 消息包括请求和响应两部分, 所有报文包含标题和其他片断 (例如 question 和 RR ，取决于报文类型)。
- DNS Message - Header: 给出语义的上下文，包括查询个数、结果个数、 会话 ID 等
- DNS Message - Question: 包含要针对 nameserver 执行的查询
- DNS Message - RR: 包装格式相同, 可以根据类型分析字段 (RDATA)

>The DNS assumes that messages will be transmitted as datagrams or in a byte stream carried by a virtual circuit. While virtual circuits can be used for any DNS activity, datagrams are preferred for queries due to their lower overhead and better performance.  — 《[RFC-1035 DOMAIN NAMES - IMPLEMENTATION AND SPECIFICATION](https://tools.ietf.org/html/rfc1035)》

![](http://riboseyim-qiniu.riboseyim.com/DNS_Package_Message.jpeg)

```sh
$ping baidu.com | head -1
PING baidu.com (220.181.57.216) 56(84) bytes of data.

$host baidu.com
baidu.com has address 220.181.57.216
baidu.com has address 123.125.115.110
```

#### unknown host

**strace** 是 Linux 环境下的一款程序调试工具，用来监视一个应用程序所使用的系统调用及它所接收的系统信息。借助 strace 我们可以更好地理解 DNS 工作原理。

```sh
$ping baidu.com
ping: unknown host baidu.com

# strace -e trace=open -f ping -c1 baidu.com
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libcap.so.2", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libidn.so.11", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libattr.so.1", O_RDONLY|O_CLOEXEC) = 3
open("/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
open("/etc/resolv.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libnss_files.so.2", O_RDONLY|O_CLOEXEC) = 4
open("/etc/host.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/hosts", O_RDONLY|O_CLOEXEC)  = 4
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libnss_dns.so.2", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libresolv.so.2", O_RDONLY|O_CLOEXEC) = 4
ping: unknown host baidu.com
+++ exited with 2 +++
```

![NSSwitch](http://riboseyim-qiniu.riboseyim.com/Linux-Works-DNS-NSSwitch.png)

#### hosts - static table lookup for hostnames

**/etc/hosts** 主机名查询静态表；主要用于IP地址与计算机主机名之间的转换。与 /etc/resolv.conf 的区别是，用户可以直接对 hosts 文件进行控制。一般情况下，我们主要通过 DNS 自动提供动态的主机名解析。不过 hosts 文件仍然是一个可以作为备用手段。

- 【IPv4】 127.0.0.1       localhost
- 【FQDN】 192.168.1.10    foo.mydomain.org   foo
- 【FQDN】 209.237.226.90  www.opensource.org
- 【IPv6】 ::1    localhost ip6-localhost ip6-loopback
- 【IPv6】 ff02::1   ip6-allnodes
- 【IPv6】 ff02::2   ip6-allrouters

#### /etc/resolv.conf

**/etc/resolv.conf** DNS 客户机配置文件，用于设置 DNS 服务器的 IP 地址及 DNS 域名，还包含了主机的域名搜索顺序。

值得注意的是，许多程序能够覆盖 /etc/resolv.conf 里的内容（例如 dhcpcd, NetworkManager ），但是有些时候我们希望能够手动设定 DNS 设置(比如使用静态IP时)，可以参考以下几种方法：
- 修改 dhcpcd 配置，echo “nohook resolv.conf” >  /etc/dhcpcd.conf
- 创建 resolv.conf.head ，dhcpcd将把这个文件插入到 /etc/resolv.conf 文件头.
- 写保护 /etc/resolv.conf，chattr +i /etc/resolv.conf

```sh
# vi /etc/resolv.conf
nameserver 1.1.1.1
```

```c
# strace -e trace=open -f ping -c1 baidu.com
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libcap.so.2", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libidn.so.11", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
open("/lib64/libattr.so.1", O_RDONLY|O_CLOEXEC) = 3
open("/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
open("/etc/resolv.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libnss_files.so.2", O_RDONLY|O_CLOEXEC) = 4
open("/etc/host.conf", O_RDONLY|O_CLOEXEC) = 4
open("/etc/hosts", O_RDONLY|O_CLOEXEC)  = 4
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libnss_dns.so.2", O_RDONLY|O_CLOEXEC) = 4
open("/lib64/libresolv.so.2", O_RDONLY|O_CLOEXEC) = 4
PING baidu.com (220.181.57.216) 56(84) bytes of data.
open("/etc/hosts", O_RDONLY|O_CLOEXEC)  = 4
64 bytes from 220.181.57.216: icmp_seq=1 ttl=128 time=174 ms
--- baidu.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 174.062/174.062/174.062/0.000 ms
+++ exited with 0 +++
```

#### NSSwitch

**nsswitch.conf**（name services switch）是 SUN 公司开发的一种扩展。每一行都标识特定类型的网络信息（如主机、口令和组）以及信息源（如 NIS+ 表、NIS 映射、DNS 主机表或本地 /etc）。

>NSSwitch is not just for DNS lookups. It’s also used for passwords and user lookup information.

![](http://riboseyim-qiniu.riboseyim.com/Linux-Works-DNS-Call.png)

```sh
# more /etc/nsswitch.conf
passwd:     files sss
shadow:     files sss
group:      files sss
hosts:      files dns
bootparams: nisplus [NOTFOUND=return] files
ethers:     files
netmasks:   files
networks:   files
protocols:  files
rpc:        files
services:   files sss
netgroup:   files sss
publickey:  nisplus
automount:  files sss
aliases:    files nisplus
```

#### dig | Domain Information Groper

**dig**（Domain Information Groper，域信息搜索器）命令是一个用于查询 DNS 的工具。dig 总共有42个查询选项，涉及到 DNS 信息的方方面面，在 DNS 问题诊断时可以将整个过程信息输出。

```sh
dig +nocmd google.com  输出过滤版本信息
dig +short google.com  输出最精简的CNAME信息和A记录
dig +nostat google.com 输出过滤统计信息
dig +trace google.com 输出跟踪，从根域查询直到最终结果
```

```sh
# dig +short google.com
74.125.130.102
74.125.130.139
74.125.130.138
74.125.130.100
74.125.130.101
74.125.130.113

# dig +trace google.com
DiG 9.9.4-RedHat-9.9.4-18.el7  +trace google.com
;; global options: +cmd
.			5	IN	NS	a.root-servers.net.
.			5	IN	NS	b.root-servers.net.
.			5	IN	NS	c.root-servers.net.
.			5	IN	NS	d.root-servers.net.
.			5	IN	NS	e.root-servers.net.
.			5	IN	NS	f.root-servers.net.
.			5	IN	NS	g.root-servers.net.
.			5	IN	NS	h.root-servers.net.
.			5	IN	NS	i.root-servers.net.
.			5	IN	NS	j.root-servers.net.
.			5	IN	NS	k.root-servers.net.
.			5	IN	NS	l.root-servers.net.
.			5	IN	NS	m.root-servers.net.
.			5	IN	RRSIG	NS 8 0 518400 20180803170000 20180721160000 41656 .
.....
;; Received 717 bytes from 192.168.213.2#53(192.168.213.2) in 10120 ms
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			86400	IN	DS	30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CF C41A5766
com.			86400	IN	RRSIG	DS 8 1 86400 20180804050000 20180722040000 41656 .
.....
;; Received 1170 bytes from 192.36.148.17#53(i.root-servers.net) in 10405 ms
google.com.		172800	IN	NS	ns2.google.com.
google.com.		172800	IN	NS	ns1.google.com.
google.com.		172800	IN	NS	ns3.google.com.
google.com.		172800	IN	NS	ns4.google.com.
.....
;; Received 772 bytes from 192.26.92.30#53(c.gtld-servers.net) in 4914 ms
google.com.		300	IN	A	172.217.31.238
;; Received 44 bytes from 216.239.36.10#53(ns3.google.com) in 1003 ms
```

#### DNS Coding API

```go
package main

import (
	"fmt"
	"net"
)

func main() {

	query("riboseyim.com")
	query("qq.com")
	query("facebook.com")

	host := "riboseyim-qiniu.riboseyim.com"
	cname, _ := net.LookupCNAME(host)
	fmt.Println("CNAME(域名),host", host, "cname", cname)

	server_ip := "8.8.8.8"
	ptr, _ := net.LookupAddr(server_ip)
	for _, ptrvalue := range ptr {
		fmt.Println("PTR(指针，IP地址的别名),server_ip", server_ip, "ptrvalue", ptrvalue)
	}

}

func query(domain string) {
	iprecords, _ := net.LookupIP(domain)
	for _, ip := range iprecords {
		fmt.Println("A(主机地址),domain", domain, "ip", ip)
	}

	nameserver, _ := net.LookupNS(domain)
	for _, ns := range nameserver {
		fmt.Println("NS(域名服务器),domain", domain, "nameserver", ns)
	}

	mxrecords, _ := net.LookupMX(domain)
	for _, mx := range mxrecords {
		fmt.Println("MX(邮件交换),domain", domain, "mx.host", mx.Host, "mx.pref", mx.Pref)
	}

	txtrecords, _ := net.LookupTXT(domain)

	for _, txt := range txtrecords {
		fmt.Println("TXT(文本标识),domain", domain, "txt", txt)
	}
}

```

## 案例

- 重点案例：ARP 与网关欺骗攻击

- [hyperfox:HTTP/HTTPs MITM proxy and traffic recorder with on-the-fly TLS cert generation. ](https://github.com/malfunkt/hyperfox)
- [arpfox](https://github.com/malfunkt/arpfox)

## Tips

#### DNS Tools

- chrome DNS Cache
```bash
# 浏览器
chrome://net-internals/#dns
# 清空缓存
“clean host cache”
```

- Windows DNS Cache
```bash
# 查看缓存
ipconfig /displaydns
# 清空缓存
ipconfig /flushd ns
```

- Mac OS X
```
$ sudo killall -HUP mDNSResponder
```

- Linux

```bash
# 清空DNS缓存，重启 nscd 进程
$ /etc/rc.d/init.d/nscd restart
```

- 网卡流量 nLoad
```bash
wget http://www.roland-riegel.de/nload/nload-0.7.2.tar.gz
tar zxvf nload-0.7.2.tar.gz
cd nload-0.7.2
./configure
make -j4
make install
```

#### bond

```bash
# modinfo bonding
vi /etc/sysconfig/network-scripts/ifcfg-bond0  
DEVICE=bond0
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.10
GATEWAY=192.168.1.1
NETMASK=255.255.255.249
USERCTL=NO

$ more ifcfg-eth0
#HWADDR=74:86:7A:DD:C3:68
DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=NO

$ more ifcfg-eth1
##HWADDR=74:86:7A:DD:C3:69
DEVICE=eth1
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=NO

more /etc/modprobe.conf
options bonding mode=1 miimon=100

```

## 拓展阅读

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### 性能诊断指南
- [Linux 性能诊断：负载评估](https://riboseyim.com/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断：快速检查单](https://riboseyim.com/2017/12/11/Linux-Perf-Netflix/)
- [Linux 性能诊断：JVM](https://riboseyim.com/2018/08/07/Linux-Perf-JVM/)

#### How Linux Works
- [How Linux Works：The Big Picture](https://riboseyim.com/2019/04/21/Linux-Works/)
- [How Linux Works：BASIC Commands](https://riboseyim.com/2017/04/26/Linux-Commands/)
- [How Linux Works：BASIC Commands Extension](https://riboseyim.com/2018/09/03/Linux-Commands-New/)
- [How Linux Works：Device and FileSystem](https://riboseyim.com/2018/06/07/Linux-Works-FileSystem/)
- [How Linux Works：Boots](https://riboseyim.com/2017/05/29/Linux-Works-Boots/)
- [How Linux Works：用户空间](https://riboseyim.com/2019/04/21/Linux-Works-User-Space/)
- [How Linux Works：内存管理](https://riboseyim.com/2017/12/11/Linux-Works-Memory/)
- [How Linux Works：网络管理](https://riboseyim.com/2018/01/08/Linux-Works-Network/)
- Preview[How Linux Works：路由管理](https://riboseyim.com/2019/03/05/Linux-Works-Router/)

#### 动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.com/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.com/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.com/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.com/2018/02/16/DTrace-Linux/)

#### 案例与实务
- [最佳工程实践：Stack Overflow 架构 - 2016 Edition](https://riboseyim.github.io/2016/07/17/OpenSource-StackOverflow/)
- [最佳工程实践：Oracle 数据库迁移割接实践](https://riboseyim.github.io/2016/06/12/Technology-Oracle/)
- [最佳工程实践：基于LVS的AAA负载均衡架构实践](https://riboseyim.github.io/2016/09/01/AAA/)  
- [VIPServer | Facebook Open-sourcing Katran, a scalable network load balancer](https://code.facebook.com/posts/1906146702752923/open-sourcing-katran-a-scalable-network-load-balancer/)


#### 扩展阅读
- [远程通信协议：从 CORBA 到 gRPC](https://riboseyim.github.io/2017/10/30/Protocol-gRPC/)
- [Wireshark 使用技巧](https://blog.csdn.net/ch853199769/article/details/78753963)


## 参考文献
- [RFC-1035 DOMAIN NAMES - IMPLEMENTATION AND SPECIFICATION](https://tools.ietf.org/html/rfc1035)
- [Writing DNS messages from scratch using Go](https://ops.tips/blog/raw-dns-resolver-in-go)
- [hosts - static table lookup for hostnames | Linux Programmer's Manual](http://man7.org/linux/man-pages/man5/hosts.5.html)
- [strace 跟踪进程中的系统调用](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/strace.html)
- [Anatomy of a Linux DNS Lookup – Part I](https://zwischenzugs.com/2018/06/08/anatomy-of-a-linux-dns-lookup-part-i/)
- [Anatomy of a Linux DNS Lookup – Part II](https://zwischenzugs.com/2018/06/18/anatomy-of-a-linux-dns-lookup-part-ii/)
- [Find DNS records programmatically](http://www.golangprograms.com/find-dns-records-programmatically.html)
- [Build a DNS server in Golang](https://medium.com/@owlwalks/build-a-dns-server-in-golang-fec346c42889)
- [网卡收包流程 | 原创 2017-03-16 信鸽工程师Henry 腾讯大数据](https://mp.weixin.qq.com/s?__biz=MzA3MDQ4MzQzMg==&mid=2665690630&idx=1&sn=89e75f7fb2910b74b082515b7107222b&chksm=842bba81b35c33978e0d127cf734e4d5c03ed8e15a857f43626b72b1ca9a1024f945e27d7f1f&mpshare=1&scene=1&srcid=0316ZJP9Ue8U7triSRjq4HhK%23rd)
- [Linux下进程/程序网络带宽占用情况查看工具 -- NetHogs](http://www.vpser.net/manage/nethogs.html)
- [每天一个linux命令（53）：route命令](http://www.cnblogs.com/peida/archive/2013/03/05/2943698.html)
