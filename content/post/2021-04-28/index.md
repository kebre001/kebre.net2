---
title: OpenBSD VMD example
description: How to use OpenBSD virtualisation
slug: openbsd-vmd-example
date: 2021-04-28 00:00:00+0000
# image: cover.png
categories:
    - OS
    - software
tags:
    - OpenBSD
    - Virtualisation
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p>vmctl - https://man.openbsd.org/vmctl<br>vmd - https://man.openbsd.org/vmd</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Start by downloading the desired OpenBSD version you want to run. In my case I wanted to run OpenBSD 6.2 which is EOL. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I found a Swedish repo that has all OpenBSD versions available so I downloaded it from here:<br>https://ftp.lysator.liu.se/pub/OpenBSD/</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I had some issues booting only from the install62.iso so I downloaded the bsd.rd and booted from it and used the iso just to install all sets.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl create -s 12G openbsd62.immg</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl start -m 4G -L -i 1 -b bsd.rd -r install62.iso -d /home/abc/vms/openbsd62.img -c openbsd62</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>After the install finishes you can boot directly from you virtual disk.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl start -m 4G -L -i 1 -d /home/abc/vms/openbsd62.img -c openbsd62</code></pre>
<!-- /wp:code -->