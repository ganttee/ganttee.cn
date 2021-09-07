---
title: "Python Xlrd不支持xlsx"
# description:
date: 2021-07-13T17:12:56+08:00
draft: false
tags:
- python
- xlrd
- xlsx
- excel
# categories:
# - ...
---

新版本的xlrd不支持读取xlsx的Excel文件了，那么只需要回退版本即可。
解决办法如下：
``` bash
pip uninstall xlrd
pip install xlrd==1.2.0
```
