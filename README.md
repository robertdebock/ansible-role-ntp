ntp
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-ntp.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-ntp)

Provides ntp for your system.

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-ntp) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-ntp/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
```
pip install molecule
molecule test --scenario-name fedora-latest
```
There are many scenarios available, please have a look in the `molecule/` directory.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/ntp.png "Dependency")

Requirements
------------

A Linux system. ;-)

Role Variables
--------------

- ntp_interfaces: A list of interfaces to listen on, for example:

```
ntp_interfaces:
  - address: 127.0.0.1
```

- ntp_restrict: A list of IP addresses and options to allow NTP traffic from:
```
ntp_restrict:
  - address: 127.0.0.1
  - address: ::1
  - address: 192.168.1.1 nomodify notrap nopeer noquery
```

- ntp_pool: A list of NTP pools and their options:
```
ntp_pool:
  - name: 2.fedora.pool.ntp.org iburst
```

- ntp_servers: A list of NTP servers and their options:

```
ntp_servers:
  - name: ntp.example.com
    options:
      - iburst
```

Dependencies
------------

You can prepare your system using the role:

- [robertdebock.bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|
|------------|-----------|-----------|-----------|
|alpine-edge|yes|yes|yes|
|alpine-latest|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-latest|yes|yes|yes|
|debian-latest|yes|yes|yes|
|debian-stable|yes|yes|yes|
|fedora-latest|yes|yes|yes|
|fedora-rawhide|yes|yes|yes|
|opensuse-leap|yes|yes|yes|
|opensuse-tumbleweed|yes|yes|yes|
|ubuntu-artful|yes|yes|yes|
|ubuntu-latest|yes|yes|yes|

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

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
