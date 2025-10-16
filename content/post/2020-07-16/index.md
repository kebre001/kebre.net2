---
title: Build custom OpenBSD
description: Building a custom OpenBSD image
slug: build-custom-openbsd
date: 2020-07-16 00:00:00+0000
# image: cover.png
categories:
    - OS
    - Software
tags:
    - OpenBSD
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p><a href="https://man.openbsd.org/release">https://man.openbsd.org/release</a></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
```bash
dd if=/dev/zero of=mydisk.img bs=1 count=0 seek=2G

vnconfig mydisk.img

fdisk -iy vnd0

disklabel -E vnd0
add slice a with all the storage

newfs /dev/vnd0a


mount /dev/vnd0a /mnt/
cd /mnt/

tar zxvfph base67.tgz

cp /etc/resolv.conf /mnt/etc
cp /etc/installurl /mnt/etc

cd /mnt/dev/
./MAKEDEV all

#If you want to make it bootable copy the kernel
cp /bsd /mnt/

chroot /mnt/ /bin/sh

#Set root password
passwd root

#Create a fstab
#sd0a

# cat /etc/boot.conf
set tty com0
stty com0 115200

 
#cat /etc/ttys
#tty00   "/usr/libexec/getty std.9600"   vt220   on secure
tty00   "/usr/libexec/getty std.115200"   vt220   on secure</code></pre>

```
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Good idea now is to bake a backup of the image.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Other good stuff to consider:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Allow ssh?</li><li>Create additional users?</li><li>No network configuration has been made yet</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2>Test the image</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>OpenBSD has <a href="https://man.openbsd.org/vmd">vmd </a>with <a href="https://man.openbsd.org/vmctl">vmctl </a>that allows you to run a virtual machine easily.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl start -m 1G -i 1 -b /bsd -d mydisk.img "myvm" -c</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul><li><strong>m </strong>- memory</li><li><strong>i </strong>- #nr of interfaces (network)</li><li><strong>b </strong>- bios/kernel</li><li><strong>d </strong>- disk</li><li><strong>c </strong>- attach console after start</li></ul>
<!-- /wp:list -->