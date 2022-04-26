---
title: 玩转编程语言:Perl
date: 2017-05-16 09:20:26
tags: [Developer]
categories: 工程技术
---
## 摘要

<!--more-->

## Tips

- /usr/bin/perl^M: bad interpreter

```perl
#### get rid of ^M with
dos2unix /usr/local/bin/sr_server
perl -pi -e 'tr[\r][]d' /usr/local/bin/sr_server
```

## 扩展阅读

- [玩转编程语言系列](https://riboseyim.github.io/2017/05/26/Language/)

## 参考文献
