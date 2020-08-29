---
title: TypeError '_Environ' object is not callable
date: 2020/08/22 17:57:00
categories:
- [python]
tags:
- [python]
- [err]
---

# 概要

```
Traceback (most recent call last):
  File "main.py", line 9, in <module>
    CALLBACK_URL   =os.environ("CALLBACK_URL")
TypeError: '_Environ' object is not callable
```

# 解決方法

環境変数を取得する()を[]に修正する

**誤**

```
RSS_URL=os.environ("RSS_URL")
```

**正**

```
RSS_URL=os.environ["RSS_URL"]
```

参考：https://github.com/MD-Studio/cerise/issues/67