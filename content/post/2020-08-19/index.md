---
title: Salt Capirca for BSD pf
description: Bake Saltstack 3001 for OpenBSD
slug: salt-capirca-for-bsd-pf
date: 2020-08-19 00:00:00+0000
# image: cover.png
categories:
    - software
tags:
    - OpenBSD
    - Saltstack
    - Salt
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
<!-- wp:paragraph -->
<p>Use Salt to generate firewall rules for Open/Free-BSD<br><a href="https://github.com/google/capirca">Google Capirca</a><br><a href="https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.capirca_acl.html">Salt capirca_acl</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is not tested yet.<br>Since the syntax of pf.conf is not exactly the same between Free/Open-BSD some functions/attributes may not work(?)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Pillar-data:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>acl:
  - my-filter:
      terms:
        - my-term:
            source_port: &#91;1234, 1235]
            action: reject
        - my-other-term:
            source_port:
              - &#91;5678, 5680]
            protocol: tcp
            action: accept</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo salt freebsd* capirca.get_policy_config packetfilter
freebsd-lab:
    # Packetfilter my-filter Policy
    # $Date: 2020/08/19 $
    # inet

    # term my-term
    block return quick inet from { any } port { 1235 1234 } to { any } flags S/SA

    # term my-other-term
    pass quick inet proto { tcp } from { any } port { 5678:5680 } to { any } flags S/SA keep state</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>How to apply to pf.conf</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">/etc/pf.conf:
  file.managed:
    - contents: {{ salt.capirca.get_policy_config('packetfilter') }} </pre>
<!-- /wp:preformatted -->