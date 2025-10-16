---
title: Custom OpenBSD package
description: How to bake a custom OpenBSD package
slug: custom-openbsd-package
date: 2020-08-03 00:00:00+0000
# image: cover.png
categories:
    - OS
    - software
tags:
    - OpenBSD
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p><a href="https://man.openbsd.org/package.5">https://man.openbsd.org/package.5</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://man.openbsd.org/pkg_create.1">https://man.openbsd.org/pkg_create.1</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="http://www.openbsd.org/papers/opencon07-portstutorial/index.html">http://www.openbsd.org/papers/opencon07-portstutorial/index.html</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://man.openbsd.org/update-plist.1">https://man.openbsd.org/update-plist.1</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>pkg_create can maybe solve the whole thing about creating a package.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You want to do everything as a port and the run 'make package'.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Explaination of variables in the ports: /usr/ports/lang/python/</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>make -V &lt;variable-name></p>
<!-- /wp:paragraph -->