---
title: OpenBSD vmm/vmd/vmctl
description: How to use OpenBSD virtualisation
slug: openbsd-vmm-vmd-vmctl
date: 2020-11-05 00:00:00+0000
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
<p><a href="https://www.openbsd.org/faq/faq16.html">https://www.openbsd.org/faq/faq16.html</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>/etc/pf.conf</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>match out on egress from 100.64.0.0/10 to any nat-to (egress)</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sysctl net.inet.ip.forwarding=1</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->