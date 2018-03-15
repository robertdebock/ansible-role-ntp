ntp
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-ntp.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-ntp)

Provides ntp for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/robertdebock.github.io/artifacts/ntp.png "Dependency")

Requirements
------------

A Linux system. ;-)

Role Variables
--------------

None known.

Dependencies
------------

You can prepare your system using the role:

- robertdebock.bootstrap

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Example Playbook
----------------

```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.ntp
```

Install this role using `galaxy install robertdebock.tomcat`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
