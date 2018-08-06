---
title: Unresolved Import Issues with PyDev and Eclipse
date: 2018-08-06 09:19:48
tags: [eclipse, python]
categories: eclipse
---

eclipse导入包内的其他脚本时会报错，需要带上包名，如

```python
#stat.py
from test.utils import extract_docs #test是包名，如果直接运行`python3 stat.py`会报错，找不到test这个包
```

解决办法是:

- Select **Project**-> RightClick-> **PyDev**-> **Remove PyDev Project Config**
- file-> **restart**

[Unresolved Import Issues with PyDev and Eclipse](https://stackoverflow.com/questions/4631377/unresolved-import-issues-with-pydev-and-eclipse)

 