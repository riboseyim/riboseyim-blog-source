---
title: 开源技术架构漫谈：Redis
date: 2018-08-07 11:33:11
tags: [Database,DevOps,Engineering]
categories: 工程技术
---
## 摘要

<!--more-->

## Introduction | Redis 简介

## Core Concept | Redis 核心概念

- [art](#)

#### Concept A
#### Concept B
#### Concept C

## Architecture | Redis 架构

## Best Practice | Redis 最佳实践

## Redis ABC

## Redis Tips

#### 密码

Redis 访问控制较弱，仅提供轻量级的认证方式，通过编辑 redis.conf 配置启用。

- 初始化密码

redis.conf 参数： requirepass
参考配置（重启 Redis 生效）： requirepass test123

- 不重启设置密码

```bash
redis 127.0.0.1:6379> config set requirepass test123
redis 127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "test123"
```

- 密码验证

```bash
redis-cli -h 192.168.1.2 -p 6379 -a test123
redis 127.0.0.1:6379> auth test123
OK
```

#### Client
- [redis client implement by golang](https://github.com/piaohao/godis)

## 扩展阅读

## 参考文献
