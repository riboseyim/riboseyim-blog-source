---
title: 玩转编程语言:Python
date: 2017-06-19 16:30:10
tags: [Developer]
categories: 工程技术
---
## 摘要

<!--more-->

## ABC
- [Learn By Example:Python Basics](https://github.com/learnbyexample/Python_Basics)

## Application

#### [远程通信协议：从 CORBA 到 gRPC](https://riboseyim.github.io/2017/10/30/Protocol-gRPC/)
- 一、远程调用技术简史:从 CORBA 到 gRPC
- 二、gRPC 简介
- 三、gRPC 示例代码


## Tips

#### 升级

- 升级 Python 2.7 提示缺少 SSL 模块

```python
$ python
>>> import ssl
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/usr/local/python27/lib/python2.7/ssl.py", line 60, in <module>
import _ssl # if we can't import it, let the error propagate
ImportError: No module named _ssl
>>>
```

- vi /src/Python-2.7.15/Modules/Setup

```bash
# Socket module helper for socket(2)
_socket socketmodule.c timemodule.c
# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
#SSL=/usr/local/ssl
_ssl _ssl.c \
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
-L$(SSL)/lib -lssl -lcrypto
```

- 重新编译

```bash
cd /src/Python-2.7.15/
$ make
$ make install
```

- 测试验证

```python
$ python
Python 2.7.15 (default, Oct 23 2018, 19:08:43)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-23)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import ssl
>>>
```

## 扩展阅读

- [玩转编程语言系列](https://riboseyim.github.io/2017/05/26/Language/)

## 参考文献

#### 算法
https://www.analyticsvidhya.com/blog/2015/09/full-cheatsheet-machine-learning-algorithms/
#### Python基础
http://datasciencefree.com/python.pdf
https://www.datacamp.com/community/tutorials/python-data-science-cheat-sheet-basics#gs.0x1rxEA
#### Numpy
https://www.dataquest.io/blog/numpy-cheat-sheet/
http://datasciencefree.com/numpy.pdf
https://www.datacamp.com/community/blog/python-numpy-cheat-sheet#gs.Nw3V6CE
https://github.com/donnemartin/data-science-ipython-notebooks/blob/master/numpy/numpy.ipynb
#### Pandas
http://datasciencefree.com/pandas.pdf
https://www.datacamp.com/community/blog/python-pandas-cheat-sheet#gs.S4P4T=U
https://github.com/donnemartin/data-science-ipython-notebooks/blob/master/pandas/pandas.ipynb
#### Matplotlib
https://www.datacamp.com/community/blog/python-matplotlib-cheat-sheet
https://github.com/donnemartin/data-science-ipython-notebooks/blob/master/matplotlib/matplotlib.ipynb
#### Scikit Learn
https://www.datacamp.com/community/blog/scikit-learn-cheat-sheet#gs.fZ2A1Jk
http://peekaboo-vision.blogspot.de/2013/01/machine-learning-cheat-sheet-for-scikit.html
https://github.com/rcompton/ml_cheat_sheet/blob/master/supervised_learning.ipynb
#### TensorFlow
https://github.com/aymericdamien/TensorFlow-Examples/blob/master/notebooks/1_Introduction/basic_operations.ipynb
#### PyTorch
https://github.com/bfortuner/pytorch-cheatsheet
