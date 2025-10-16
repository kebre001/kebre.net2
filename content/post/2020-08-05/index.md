---
title: Saltstack 3001 on OpenBSD
description: Bake Saltstack 3001 for OpenBSD
slug: saltstack-3001-on-openbsd
date: 2020-08-05 00:00:00+0000
# image: cover.png
categories:
    - OS
    - software
tags:
    - OpenBSD
    - Saltstack
    - Salt
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p>Main goal is to run Salt version 3001 on OpenBSD 6.6</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Today it looks like the port/package for version 3001 will only be released to OpenBSD 6.7 and forward(?).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So how do I make a port/package to run version 3001 on my OpenBSD 6.6 systems.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To learn more about the 3001 release I would recommend reading: <a href="https://salt.tips/whats-new-in-salt-sodium/">https://salt.tips/whats-new-in-salt-sodium/</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Procedure</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>Download ports</li><li>Make a copy of the old port / create a port</li><li></li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Download ports</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Start by downloading the OpenBSD ports on a fresh 6.6: <a href="https://www.openbsd.org/faq/ports/ports.html#PortsFetch">https://www.openbsd.org/faq/ports/ports.html#PortsFetch</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Just to keep a copy of the commands that I followed:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>cd /tmp
ftp https://cdn.openbsd.org/pub/OpenBSD/$(uname -r)/{ports.tar.gz,SHA256.sig}
signify -Cp /etc/signify/openbsd-$(uname -r | cut -c 1,3)-base.pub -x SHA256.sig ports.tar.gz

cd /usr
tar xzf /tmp/ports.tar.gz</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>now all ports is available in /usr/ports/ and Salt is located in /usr/ports/sysutils/salt</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Create the new port</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>What I did was to create a copy of of the existing port and then just updated all the dependencies.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>My test setup using vmd/vmctl</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Testing the package I used the OpenBSD vmd, vmctl</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To keep it simple:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>on the mother machine update pf.conf with the following lines:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>set skip on tap0
set skip on tap1

pass out on egress from 100.64.0.0/8 to any nat-to (egress)</code></pre>
<!-- /wp:code -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">The host (tapX) address is assigned 100.64.n.2, where 'n' is the numeric VM ID visible in the 'vmctl status' command
The guest (vio0) address is assigned 100.64.n.3</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size"><a href="https://man.openbsd.org/vmctl#LOCAL_INTERFACES">https://man.openbsd.org/vmctl#LOCAL_INTERFACES</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>tap0/1 is the interfaces vmctl will create for the VM and the other rule is to allow the VM's to communicate with internet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You will have to enable vmd before working with vmctl:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>rcctl -f start vmd (or add it to /etc/rc.local, -f is to force it once)</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Create a virtual disk</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl create -s 2G disk.img</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Then start the new VM with a few options:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>vmctl start -c -m 2G -L -r install66.iso -d disk.img "myvm"</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>All the flags/parameters can be found at <a href="https://man.openbsd.org/vmctl">vmctl</a> (<a href="https://man.openbsd.org/vmctl">https://man.openbsd.org/vmctl</a>) but -c is to attach to the console directly. Remove it to start the VM in the background and use vmctl to attach to it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will launch the regular OpenBSD installation script to setup your VM. If you want to make it easier for next time you can shutdown the VM when the installation is completed and copy the file disk.img. This will be like taking a snapshot.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When the VM has booted I update the <em>/etc/resolv.conf</em> to a public DNS like 8.8.8.8/9.9.9.9 just to be able to fetch the package dependencies from the internet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now download the package from the mother machine:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>scp &lt;username>@100.64.n.2:/usr/ports/packages/amd64/all/salt-3001p0.tgz .</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This will download the package to the current directory. Replace the dot (.) with the path you want to place the file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now the package can be installed by running:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>pkg_add -D unsigned salt-3001p0.tgz</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>I have not looked into why the package is unsigned but adding "-D unsigned" ignores that error/issue.</p>
<!-- /wp:paragraph -->