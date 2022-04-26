---
title: DevOps-Puppet
date: 2018-09-17 15:34:02
tags:
categories: 工程技术
---
## 摘要

<!--more-->



## Introduction | Puppet 简介

#### Install
```bash
$ wget http://downloads.puppetlabs.com/

## hello puppet !
$ puppet status
{
  "is_alive": true,
  "version": "5.5.6"
}

##
Puppet does not support OS X versions < 10.4
```

## Core Concept | Puppet 核心概念

#### Concept A
#### Concept B
#### Concept C

## Architecture | Puppet 架构

#### Puppet Development Kit

>PDK provides integrated testing tools and a simple command line interface to help you develop, validate, and test modules.

#### Task Runner : Bolt

>Bolt is an open source task runner that automates the manual work that you do to maintain your infrastructure. Use Bolt to automate tasks that you perform on your infrastructure on an as-needed basis, for example, when you troubleshoot a system, deploy an application, or stop and restart services. Bolt connects directly to remote nodes with SSH or WinRM, so you are not required to install any agent software.

## Best Practice | Puppet 最佳实践

## Tips
```bash      
# Yum   
$ yum clean all
$ yum update yum
$ yum update
# MacPorts
$ wget https://guide.macports.org/
$ port version
Version: 2.5.3
```

## 参考文献
