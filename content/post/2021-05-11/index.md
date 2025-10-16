---
title: Mirror a OpenBSD repo
description: How to setup a mirror of OpenBSD repo
slug: mirror-openbsd-repo
date: 2021-05-11 00:00:00+0000
# image: cover.png
categories:
    - OS
tags:
    - OpenBSD
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
# Mirror a OpenBSD repo

http://www.openbsd.org/ftp.html#prospective

<!-- wp:code -->
```
rsync -rv --progress --delete-delay --delay-updates --fuzzy rsync://openbsd.ipacct.com/OpenBSD/6.2/packages/amd64/ /var/www/pub/OpenBSD/6.2/packages
```

OpenBSd httpd config:

http://www.openbsd.org/httpd.conf

Source: https://daulton.ca/2018/10/openbsd-create-private-mirror/


https://webhome.phy.duke.edu/~rgb/General/yum_article/yum_article/node15.html

Only to list the files on the remote hosts since the rsync and http endpoints may contains different files:

```
rsync --list-only rsync://openbsd.ipacct.com/OpenBSD/6.2/packages/amd64/
```

https://stackoverflow.com/questions/13414086/how-to-use-rsync-list-only-source-to-list-all-the-files-in-that-directory