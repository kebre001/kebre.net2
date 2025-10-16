---
title: FreeBSD mfsbsd pxe
description: PXE boot FreeBSD
slug: freebsd-mfsbsd-pxe
date: 2021-08-31 00:00:00+0000
# image: cover.png
categories:
    - OS
tags:
    - FreeBSD
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p><a href="https://github.com/mmatuska/mfsbsd">https://github.com/mmatuska/mfsbsd</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://forums.freebsd.org/threads/freebsd-iso-bootable.52444/">https://forums.freebsd.org/threads/freebsd-iso-bootable.52444/</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://docs.freebsd.org/en/books/handbook/disks/#disks-virtual">https://docs.freebsd.org/en/books/handbook/disks/#disks-virtual</a></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>fetch http://ftp4.se.freebsd.org/pub/FreeBSD/releases/ISO-IMAGES/13.0/FreeBSD-13.0-RELEASE-amd64-disc1.iso

mdconfig -a -t vnode -f <meta charset="utf-8">FreeBSD-13.0-RELEASE-amd64-disc1.iso
mount_cd9660 /dev/md0 /cdrom/

<meta charset="utf-8">git clone https://github.com/mmatuska/mfsbsd
cd mfsbsd

#Customize wanted files in conf/ specially rc.local that kickstarts everything

make iso BASE=/cdrom/usr/freebsd-dist
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>rc.local</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>fetch http://&lt;servername&gt;/mfsbsd/installerconfig -o /etc/installerconfig
tail -n 7 /etc/rc.local &gt; /tmp/start.sh
chmod +x /tmp/start.sh
/tmp/start.sh
exit 0

#!/bin/csh
setenv DISTRIBUTIONS "kernel.txz base.txz"
setenv BSDINSTALL_DISTDIR /tmp
setenv BSDINSTALL_DISTSITE http://ftp4.se.freebsd.org/pub/FreeBSD/releases/amd64/13.0-RELEASE

bsdinstall distfetch
bsdinstall script /etc/installerconfig</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>installerconfig, ensure that gpart edits correct harddrive</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>PARTITIONS=ada0
DISTRIBUTIONS="kernel.txz base.txz"
BSDINSTALL_DISTDIR=/tmp
BSDINSTALL_DISTSITE=http://ftp4.se.freebsd.org/pub/FreeBSD/releases/amd64/13.0-RELEASE

#!/bin/sh
gpart bootcode -b /boot/pmbr    -p /boot/gptboot -i 1 ada0
sysrc ifconfig_em0=DHCP
sysrc sshd_enable=YES
echo "Installation complete, running in host system"
echo "hostname=\"FreeBSD\"" &gt;&gt; /etc/rc.conf
echo "autoboot_delay=\"5\"" &gt;&gt; /boot/loader.conf
echo "sshd_enable=YES" &gt;&gt; /etc/rc.conf
echo "Setup done" &gt;&gt; /tmp/log.txt
echo "Setup done."
poweroff</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>ipxe file</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>#!ipxe
sanboot http://XXXXXX/mfsbsd-13.0-RELEASE-amd64.iso
boot</code></pre>
<!-- /wp:code -->