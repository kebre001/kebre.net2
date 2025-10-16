---
title: Build Python 3.X on OpenBSD
description: Build Python 3 for OpenBSD
slug: build-python-3-on-openbsd
date: 2021-04-29 00:00:00+0000
# image: cover.png
categories:
    - OS
    - software
tags:
    - OpenBSD
    - Python
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
```
cd /usr/ports/lang/python/3.X/
make plist
make package

Maybe
env PKG_CREATE_NO_CHECKS=yes make package</code></pre>
```

Ensure that you dont have a current Python version that may conflict

#thisisugly