---
title: Simple testing of Salt modules
description: Test Saltstack modules
slug: testing-of-salt-modules
date: 2023-08-14 00:00:00+0000
# image: cover.png
categories:
    - Software
tags:
    - Saltstack
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
# Simple testing of Salt modules
After spending some time trying to learn testing and later how to test Salt modules I felt that I need to summarise what I have done and what I missed.

Testing Salt modules seems to require the following python packages:

```
pytest
pytest-salt-factories
```

When I tried to run tests without the salt-factories plugin I only received the following error:

```
FAILED _modules/test_interface_freebsd.py::test_parse_rc_conf - NameError: name '__salt__' is not defined
```

Since I started using Tox for testing I had forgotten to add pytest-salt-factories as a dependency for the test python environment.

```
def test_parse_rc_conf() -> None:
    rc_contents = 'ifconfig_vtnet0="inet 192.168.1.1/24"\nifconfig_vlan123="vlandev vtnet0 vlan 123 192.168.2.1/24 description \'UnitTesting\''
    with patch.dict(
        interfacemod.__salt__,
        {
            "file.replace": MagicMock(return_value=True),
            "file.read": mock_open(read_data=rc_contents),
            "file.grep": MagicMock(return_value={"stdout": rc_contents})
        },
    ):
        expected = {
            'vlan123': '"vlandev vtnet0 vlan 123 192.168.2.1/24 description \'UnitTesting\'',
            'vtnet0': '"inet 192.168.1.1/24"'
        }
        assert interfacemod.parse_rc_conf() == expected
```