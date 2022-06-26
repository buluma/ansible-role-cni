# [cni](#cni)

Ansible role to install CNI (Container Network Interface).

|GitHub|GitLab|Quality|Downloads|Version|Issues|Pull Requests|
|------|------|-------|---------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-cni/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-cni/actions)|[![gitlab](https://gitlab.com/buluma/ansible-role-cni/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-cni)|[![quality](https://img.shields.io/ansible/quality/)](https://galaxy.ansible.com/buluma/cni)|[![downloads](https://img.shields.io/ansible/role/d/)](https://galaxy.ansible.com/buluma/cni)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-cni.svg)](https://github.com/buluma/ansible-role-cni/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-cni.svg)](https://github.com/buluma/ansible-role-cni/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-cni.svg)](https://github.com/buluma/ansible-role-cni/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
```yaml
---
# Copyright (C) 2021 Robert Wimmer
# SPDX-License-Identifier: GPL-3.0-or-later

- hosts: all
  # remote_user: vagrant
  become: true
  gather_facts: true
  tasks:
    - block:
        - name: (Archlinux) Init pacman
          raw: |
            pacman-key --init
            pacman-key --populate archlinux
          changed_when: false
          ignore_errors: true

        - name: (Archlinux) Update pacman cache
          community.general.pacman:
            update_cache: yes
          changed_when: false
      when: ansible_distribution | lower == 'archlinux'

    - name: (Ubuntu) Update APT package cache
      ansible.builtin.apt:
        update_cache: "true"
        cache_valid_time: 3600
      when: ansible_distribution | lower == 'ubuntu'

- hosts: all
  # remote_user: vagrant
  become: true
  gather_facts: true
  tasks:
    - name: Include CNI role
      ansible.builtin.include_role:
        name: buluma.cni
      vars:
        cni_restart_kubelet: false
```


## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:
```yaml
---

# CNI plugin version
cni_version: "1.0.1"

# CNI binary directory
cni_bin_directory: "/opt/cni/bin"

# CNI configuration directory
cni_conf_directory: "/etc/cni/net.d"

# Directory to store the archive
cni_tmp_directory: "{{ lookup('env', 'TMPDIR') | default('/tmp',true) }}"

# Owner/group of "CNI" files/directories. If the variables are not set
# the resulting binary will be owned by the current user.
cni_owner: "root"
cni_group: "root"

# Specifies the permissions of the "CNI" binaries
cni_binary_mode: "0755"

# Operarting system
# Possible options: "linux", "windows"
cni_os: "linux"

# Processor architecture "CNI" should run on.
# Other possible values: "arm", "arm64", "mips64le", "ppc64le", "s390x"
cni_arch: "amd64"

# Name of the archive file name
cni_archive: "cni-plugins-{{ cni_os }}-{{ cni_arch }}-v{{ cni_version }}.tgz"

# The CNI download URL (normally no need to change it)
cni_url: "https://github.com/containernetworking/plugins/releases/download/v{{ cni_version }}/{{ cni_archive }}"

# Restart "kubelet" service after "CNI" binaries or configuration have changed.
# This handler expects a systemd service called "kubelet.service".
cni_restart_kubelet: false
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-cni/blob/main/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-bootstrap)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-epel)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-cni/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|archlinux|all|
|ubuntu|bionic, focal|

The minimum version of Ansible required is 2.1, tests have been done to:

- The previous version.
- The current version.
- The development version.



If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-cni/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-cni/blob/master/CHANGELOG.md)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[Michael Buluma](https://buluma.github.io/)
