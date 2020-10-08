# [ntp](#ntp)

Install and configure ntp on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-ntp.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-ntp)|[![github](https://github.com/robertdebock/ansible-role-ntp/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-ntp/actions)|[![quality](https://img.shields.io/ansible/quality/23988)](https://galaxy.ansible.com/robertdebock/ntp)|[![downloads](https://img.shields.io/ansible/role/d/23988)](https://galaxy.ansible.com/robertdebock/ntp)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-ntp.svg)](https://github.com/robertdebock/ansible-role-ntp/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.cron
    - role: robertdebock.ntp
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
```

For verification `molecule/resources/verify.yml` runs after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    _ntp_check_packages:
      default:
        - ntpstat
      Amazon-2: []
      RedHat: []
    ntp_check_packages: "{{ _ntp_check_packages[ansible_distribution ~ '-' ~ ansible_distribution_major_version] | _ntp_check_packages[ansible_os_family ~ '-' ~ ansible_distribution_major_version] | default(_ntp_check_packages[ansible_os_family] | default(_ntp_check_packages['default']))) }}"
    _ntp_check_command:
      default: ntpstat
      Amazon-2: chronyc tracking
      RedHat-8: chronyc tracking
    ntp_check_command: "{{ _ntp_check_command[ansible_distribution ~ '-' ~ ansible_distribution_major_version] | _ntp_check_command[ansible_os_family ~ '-' ~ ansible_distribution_major_version] | default(_ntp_check_command[ansible_os_family] | default(_ntp_check_command['default']))) }}"
    _ntp_success_output:
      default: "synchronised to NTP server"
      Amazon-2: "Leap status     : Normal"
      RedHat-8: "Leap status     : Normal"
    ntp_success_output: "{{ _ntp_success_output[ansible_distribution ~ '-' ~ ansible_distribution_major_version] | _ntp_success_output[ansible_os_family ~ '-' ~ ansible_distribution_major_version] | default(_ntp_success_output[ansible_os_family] | default(_ntp_success_output['default'])) }}"

  tasks:
    - name: install ntp check packages
      package:
        name: "{{ ntp_check_packages }}"
        state: present
      register: ntp_install_ntp_check_packages

    - name: check if time is synchronised
      command: "{{ ntp_check_command }}"
      register: npt_check_time_synchronised
      failed_when:
        - ntp_success_output not in npt_check_time_synchronised.stdout
      changed_when: no
      until: npt_check_time_synchronised is succeeded
      retries: 6

    - name: show output
      debug:
        msg: "{{ npt_check_time_synchronised }}"

    - name: uninstall ntp check packages
      package:
        name: "{{ ntp_check_packages }}"
        state: absent
      when:
        - ntp_install_ntp_check_packages is changed
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for ntp

# A list of IP addresses to listen on.
ntp_interfaces:
  - address: 127.0.0.1

# A list of IP addresses and options to allow NTP traffic from.
ntp_restrict:
  - address: 127.0.0.1
  - address: ::1
#  - address: 192.168.1.1 nomodify notrap nopeer noquery

# A list of NTP pools and their options.
ntp_pool:
  - name: 0.pool.ntp.org iburst
  - name: 1.pool.ntp.org iburst
  - name: 2.pool.ntp.org iburst
  - name: 3.pool.ntp.org iburst

# A list of NTP servers and their options.
# ntp_server:
#   - name: ntp.example.com
#     options:
#       - iburst

# The timezone.
ntp_timezone: Europe/Amsterdam
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.cron

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/ntp.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|amazon|2018.03|
|el|7, 8|
|debian|buster, bullseye|
|fedora|31, 32|
|ubuntu|focal, bionic, xenial|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| alpine | /lib/rc/sh/openrc-run.sh: line 100: can't create /sys/fs/cgroup/systemd/tasks: Read-only file system |
| opensuse | ConditionVirtualization=!container was not met |
| debian:buster | Unable to restart service ntp: Job for ntp.service failed because the control process exited with error code. |


## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-ntp) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-ntp/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0

## [Contributors](#contributors)

I'd like to thank everybody that made contributions to this repository. It motivates me, improves the code and is just fun to collaborate.

- [cadusk](https://github.com/cadusk)

## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
