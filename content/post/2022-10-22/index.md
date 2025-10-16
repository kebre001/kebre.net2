---
title: Newer FreeBSD version for package X bypass
description: How to install a later version of a package on FreeBSD
slug: newer-freebsd-version-for-package-x-bypass
date: 2022-10-22 00:00:00+0000
# image: cover.png
categories:
    - Software
    - OS
tags:
    - FreeBSD
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
# Newer FreeBSD version for package bypass

```
Newer FreeBSD version for package pkg:
To ignore this error set IGNORE_OSVERSION=yes
- package: 1301000
- running kernel: 1300139
```

Dont agree

Do the following change and lock your FreeBSD version to latest release for that version.

```
root@freebsd:/usr/local/etc/pkg/repos # cat FreeBSD.conf
FreeBSD: {
  url: "pkg+http://pkg0.bme.freebsd.org/FreeBSD:13:amd64/release_1/"
}
```

## Install Salt 3005.1 FreeBSD 13.0

Follow above steps first

```
pkg install python3.X
mkdir salt_env
python3.x -m venv env
source env/bin/activate.csh
pip install --upgrade pip

pip install salt==3005.1 WILL FAIL

setenv C_INCLUDE_PATH /usr/local/include/
setenv CPLUS_INCLUDE_PATH /usr/local/include/
pkg install cppzmq

might need
pkg install libunwind
```

This solved errors like:

```
/usr/include/string.h:95:9: note: previous declaration is here
      size_t   strlcpy(char * __restrict, const char * __restrict, size_t);
               ^
      1 error generated.
      error: command 'c++' failed with exit status 1
```

```
#include &lt;libunwind.h>
               ^~~~~~~~~~~~~
      1 error generated.
      error: command 'c++' failed with exit status 1
```