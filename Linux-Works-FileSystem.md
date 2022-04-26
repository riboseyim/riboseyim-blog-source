---
title: How Linux Works:Device and FileSystem
date: 2018-06-07 17:45:08
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要

- BASIC Command
- LVM
- iSCSI

<!--more-->

>传统的计算机，其工作的原理都是冯·诺依曼体系：机器里面有一个很大的存储器，用来储存所有的信息，还有一个中央区域，执行简单的计算。我们从存储器的这个地方提取一个数据，又从存储器的另一个地方提取一个数据，把这两个数据送到中央算术单元进行相加，然后把计算结果传送到存储器的另一个地方。这样来看，计算机有一个高效运转的中央处理器，工作十分卖力，速度也很快。

>相比之下，整个存储器从头到尾待在一旁，很清闲，就像是一个卡片档案柜，除了偶尔翻找几张卡片，档案柜大多数时间都闲置着。显然，如果有更多的处理器同时工作的话，我们的计算速度就能更快一些。问题是当你使用这个处理器时，可能要用到存储器的某个信息，而同时另一个处理器也需要这个信息，机器就会陷入一片混乱中。出于这些原因，大家普遍认为让很多处理器同时工作是个难题。 -- 理查德·费曼. 发现的乐趣 (未读·探索家) (Kindle 位置 470-472).

## Command

- lsblk

```sh
NAME                                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                                    8:0    0 1000G  0 disk
|-sda1                                 8:1    0  500M  0 part /boot
|-sda2                                 8:2    0 19.5G  0 part
| |-vg_nwtest68-lv_root (dm-0)       253:0    0 47.5G  0 lvm  /
| `-vg_nwtest68-lv_swap (dm-1)       253:1    0    2G  0 lvm  [SWAP]
`-sda3                                 8:3    0  980G  0 part
  |-vg_nwtest68-lv_root (dm-0)       253:0    0 47.5G  0 lvm  /
  |-vg_nwtest68-lv_data (dm-2)     253:2    0  100G  0 lvm  /data
sr0                                   11:0    1  3.6G  0 rom  /media/RHEL-6.8 Server.x86_64

```

- fdisk

The fdisk command below will print the partition table of all mounted block devices.[>>>>> More details <<<<<](https://www.tecmint.com/fdisk-commands-to-manage-linux-disk-partitions/)

```bash
$ sudo fdisk -l
[root@NW-DD-APP ~]# fdisk -l

Disk /dev/sda: 1073.7 GB, 1073741824000 bytes
255 heads, 63 sectors/track, 130541 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0008ca86

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          64      512000   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              64        2611    20458496   8e  Linux LVM
/dev/sda3            2611      130541  1027599062+  83  Linux

Disk /dev/mapper/vg_nwtest68-lv_root: 51.0 GB, 51011125248 bytes
255 heads, 63 sectors/track, 6201 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000


Disk /dev/mapper/vg_nwtest68-lv_swap: 2147 MB, 2147483648 bytes
255 heads, 63 sectors/track, 261 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000


Disk /dev/mapper/vg_nwtest68-lv_data: 107.4 GB, 107374182400 bytes
255 heads, 63 sectors/track, 13054 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000
```

- sfdisk
```sh
$ sudo sfdisk -l

Disk /dev/sda: 130541 cylinders, 255 heads, 63 sectors/track
Units = cylinders of 8225280 bytes, blocks of 1024 bytes, counting from 0

   Device Boot Start     End   #cyls    #blocks   Id  System
/dev/sda1   *      0+     63-     64-    512000   83  Linux
/dev/sda2         63+   2610-   2547-  20458496   8e  Linux LVM
/dev/sda3       2610+ 130540  127931- 1027599062+  83  Linux
/dev/sda4          0       -       0          0    0  Empty

Disk /dev/mapper/vg_nwtest68-lv_root: 6201 cylinders, 255 heads, 63 sectors/track
Disk /dev/mapper/vg_nwtest68-lv_swap: 261 cylinders, 255 heads, 63 sectors/track
Disk /dev/mapper/vg_nwtest68-lv_data: 13054 cylinders, 255 heads, 63 sectors/track

```

- cfdisk
```sh
cfdisk (util-linux-ng 2.17.2)

    Disk Drive: /dev/sda
Size: 1073741824000 bytes, 1073.7 GB
Heads: 255   Sectors per Track: 63   Cylinders: 130541

Name                    Flags                 Part Type            FS Type                         [Label]                      Size (MB)
-----------------------------------------------------------------------------------------------------------------------------------------------------------
Pri/Log             Free Space                                                        1.05            *
sda1                    Boot                   Primary             Linux ext3                                                      524.29            *
sda2                                           Primary             Linux LVM                                                     20949.50            *
sda3                                           Primary             Linux                                                       1052261.44            *
Pri/Log             Free Space                                                        5.55            *

```

## LVM

```perl
  -- create partitions on disk drives (type 8e in fdsik)
  -- create physical volumes from the partitions                 
     --> $ sudo pvcreate /dev/sda1
  -- create the volumes group                                     
     --> $ sudo vgcreate -s 16m vg /dev/sda1
  -- allocate logical volumes from the volume group               
     --> $ sudo lvcreate -l 50g -n mylvm vg
  -- format the logical volumes                                   
     --> $ sudo mkfs -t ext4 /dev/vg/mylvm
  -- mount the logical volumes (also update /etc/fstab as needed)
     --> $ mkdir /mylvm,

then --> $ sudo mount /dev/vg/mylvm /mylvm,
then add --> /dev/vg/mylvm /mylvm ext4 defaults 0 0  to the /etc/fstab
```

#### 查看

```perl

cat /etc/fstab
df -T -h  

# 查看卷组信息
vgdisplay

# 查看逻辑卷信息
lvdispaly

# lsblk
NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                              8:0    0  500G  0 disk
├─sda1                           8:1    0  500M  0 part /boot
├─sda2                           8:2    0 19.5G  0 part
│ ├─vg_nwtest68-lv_root (dm-0) 253:0    0   35G  0 lvm  /
│ └─vg_nwtest68-lv_swap (dm-1) 253:1    0    2G  0 lvm  [SWAP]
├─sda3                           8:3    0   20G  0 part
│ └─vg_nwtest68-lv_root (dm-0) 253:0    0   35G  0 lvm  /
└─sda4                           8:4    0   10G  0 part

```

#### 卸载

```perl
umount  /home

# 删除逻辑卷，注意：检查目录是否为空

lvremove  /dev/mapper/VolGroup-lv_home     
```

#### 创建

```perl
#在卷组 (name:VolGroup) 上创建逻辑卷

lvcreate -L 375GB -n lv_slview VolGroup     

# 格式化新建的逻辑卷
mkfs.ext4 /dev/mapper/VolGroup-lv_slview

# 创建挂载点
mkdir /slview

# 挂载
mount /dev/mapper/VolGroup/lv_slview /slview

# 修改 fstab 重启后自动挂载（风险点）
vi /etc/fstab

# reboot

# fstab 异常处理
mount -o remount rw /.
```

#### 扩容

```go

fdisk -l

# df -kh
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root
                       35G   23G  9.9G  70% /
tmpfs                 932M  208K  932M   1% /dev/shm
/dev/sda1             477M   41M  411M  10% /boot
/dev/mapper/VolGroup-lv_oradata1
                      2.5G  3.8M  2.4G   1% /oradata1

# pvs
  PV         VG          Fmt  Attr PSize  PFree
  /dev/sda2  VolGroup lvm2 a--u 19.51g    0
  /dev/sda3  VolGroup lvm2 a--u 19.99g    0

# pvcreate /dev/sda4
  Physical volume "/dev/sda4" successfully created

# pvs
  PV         VG          Fmt  Attr PSize  PFree
/dev/sda2  VolGroup lvm2 a--u 19.51g     0
/dev/sda3  VolGroup lvm2 a--u 19.99g     0
/dev/sda4              lvm2 ---- 10.00g 10.00g

# vgdisplay
  --- Volume group ---
  VG Name               VolGroup
  ......
  Act PV                2
  VG Size               39.50 GiB
  PE Size               4.00 MiB
  Total PE              10112
  Alloc PE / Size       10112 / 39.50 GiB
  Free  PE / Size       0 / 0   

# vgextend VolGroup /dev/sda4
  Volume group "VolGroup" successfully extended

# vgdisplay
  --- Volume group ---
  VG Name               VolGroup
  ......
  Act PV                3
  VG Size               49.50 GiB
  PE Size               4.00 MiB
  Total PE              12672
  Alloc PE / Size       10112 / 39.50 GiB
  Free  PE / Size       2560 / 10.00 GiB

# lvextend -L +10GB /dev/mapper/VolGroup-lv_oradata1
Size of logical volume VolGroup/lv_oradata1 changed from 2.50 GiB (640 extents) to 12.50 GiB (3200 extents).
  Logical volume lv_oradata1 successfully resized.

# vgdisplay
  --- Volume group ---
  VG Name               VolGroup
  ......
  Act PV                3
  VG Size               49.50 GiB
  PE Size               4.00 MiB
  Total PE              12672
  Alloc PE / Size       12672 / 49.50 GiB
  Free  PE / Size       0 / 0   

# resize2fs /dev/mapper/VolGroup-lv_oradata1
resize2fs 1.41.12 (17-May-2010)
Filesystem at /dev/mapper/VolGroup-lv_oradata1 is mounted on /oradata1;
on-line resizing required
old desc_blocks = 1, new_desc_blocks = 1
Performing an on-line resize of /dev/mapper/vg_nwtest68-lv_oradata1 to 3276800 (4k) blocks.
The filesystem on /dev/mapper/vg_nwtest68-lv_oradata1 is now 3276800 blocks long.

# df -kh
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root
                       35G   23G  9.9G  70% /
tmpfs                 932M  288K  932M   1% /dev/shm
/dev/sda1             477M   41M  411M  10% /boot
/dev/mapper/VolGroup-lv_oradata1
                       13G  6.3M   12G   1% /oradata1
```

#### 移除

- How to remove missing PV from VG

```sh
# vgreduce  --removemissing datavg
  WARNING: Device for PV fPFkBx-lbnG-R6Zo-3kq5-KOLA-U1ou-LdNIMD not found or rejected by a filter.
  Couldn\'t find device with uuid fPFkBx-lbnG-R6Zo-3kq5-KOLA-U1ou-LdNIMD.
  Wrote out consistent volume group datavg.
```

## Tips

#### SCSI

小型计算机系统接口（SCSI，Small Computer System Interface）是一种用于计算机及其周边设备之间（硬盘、软驱、光驱、打印机、扫描仪等）系统级接口的独立处理器标准。最大部分的应用是在存储设备上（例如硬盘、磁带机）。

iSCSI（Internet Small Computer System Interface，发音为/ˈаɪskʌzi/），Internet小型计算机系统接口，又称为IP-SAN，是一种基于因特网及SCSI-3协议下的存储技术，由IETF提出，并于2003年2月11日成为正式的标准。

```bash
# iqn
vi /etc/iscsi/initiatorname.iscsi

# 查找iSCSI目标
$ iscsiadm -m discovery -t st -p <组IP地址>:3260
# 登录
$ iscsiadm -m node -T <完整的目标名称,iqn.xxxxxx> -l -p <组IP>:3260
# 注销
$ iscsiadm -m node -u -T <完整的目标名称>-p <组IP地址>:3260
```
#### 异常处理

```bash
# df -F ufs -o i
Filesystem             iused   ifree  %iused  Mounted on
/dev/dsk/c0t0d0s0     190397 6443139     3%   /
/dev/dsk/c0t0d0s4     457204  165964    73%   /var
/dev/dsk/c0t1d0s3      43539 6461869     1%   /slview
/dev/dsk/c0t0d0s3      19745 1226591     2%   /oracle
# find /var/spool/clientmqueue/ -exec ls {} + |awk '{print $1}' | wc -l
  866350
# find /var/spool/clientmqueue/ -exec ls {} + |awk '{print $1}' | wc -l^C
# getconf ARG_MAX
1048320
```

## Projects
- [go-fastdfs | 一个简单的分布式文件存储，具有高性能，高可靠，免维护等优点，支持断点续传，分块上传，小文件合并，自动同步，自动修复](https://github.com/sjqzhang/go-fastdfs)
- [Fast directory traversal for Golang](https://github.com/karrick/godirwalk)
- [IPFS The Interplanetary File System | Simply Explained](https://achainofblocks.com/2018/10/05/ipfs-interplanetary-file-system-simply-explained/)

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### 扩展阅读：性能诊断指南
- [Linux 性能诊断：负载评估](https://riboseyim.com/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断：快速检查单](https://riboseyim.com/2017/12/11/Linux-Perf-Netflix/)
- [Linux 性能诊断：JVM](https://riboseyim.com/2018/08/07/Linux-Perf-JVM/)

#### 扩展阅读：How Linux Works
- [How Linux Works：The Big Picture](https://riboseyim.com/2019/04/21/Linux-Works/)
- [How Linux Works：BASIC Commands](https://riboseyim.com/2017/04/26/Linux-Commands/)
- [How Linux Works：BASIC Commands Extension](https://riboseyim.com/2018/09/03/Linux-Commands-New/)
- [How Linux Works：Device and FileSystem](https://riboseyim.com/2018/06/07/Linux-Works-FileSystem/)
- [How Linux Works：Boots](https://riboseyim.com/2017/05/29/Linux-Works-Boots/)
- [How Linux Works：用户空间](https://riboseyim.com/2019/04/21/Linux-Works-User-Space/)
- [How Linux Works：内存管理](https://riboseyim.com/2017/12/11/Linux-Works-Memory/)
- [How Linux Works：网络管理](https://riboseyim.com/2018/01/08/Linux-Works-Network/)
- Preview[How Linux Works：路由管理](https://riboseyim.com/2019/03/05/Linux-Works-Router/)

#### 扩展阅读：动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.com/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.com/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.com/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.com/2018/02/16/DTrace-Linux/)

## 参考文献
- [10 Useful Commands to Collect System and Hardware Information in Linux](https://www.tecmint.com/commands-to-collect-system-and-hardware-information-in-linux/)
- [An introduction to Linux's EXT4 filesystem](https://opensource.com/article/17/5/introduction-ext4-filesystem)
- [How to Increase the size of a Linux LVM by adding a new disk](https://www.rootusers.com/how-to-increase-the-size-of-a-linux-lvm-by-adding-a-new-disk/)
- [Unable to add physical volume '/dev/hda' to volume group](https://askubuntu.com/questions/934947/unable-to-add-physical-volume-dev-hda-to-volume-group)
- [Linux LVM硬盘管理及LVM扩容](http://www.cnblogs.com/gaojun/archive/2012/08/22/2650229.html)
- [创建和挂载 Oracle Solaris 文件系统](https://docs.oracle.com/cd/E26926_01/html/E25884/fscreate-6.html)
- [iSCSI存储技术全攻略|2007](http://www.sansky.net/article/2007-12-03-iscsi-storage.html)
- [exFAT 文件系统指南](http://www.ruanyifeng.com/blog/2018/10/exfat.html)
- [Virtual filesystems in Linux: Why we need them and how they work](https://opensource.com/article/19/3/virtual-filesystems-linux)
- [IPFS The Interplanetary File System | Simply Explained](https://achainofblocks.com/2018/10/05/ipfs-interplanetary-file-system-simply-explained/)
