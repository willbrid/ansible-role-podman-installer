# Ansible-role-podman-installer

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-podman-installer/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-podman-installer/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-podman-installer/actions/workflows/ci.yml)

The **ansible-role-podman-installer** role allows you to install the **podman** container manager for Red Hat-based distributions (RHEL, CentOS, Rocky Linux) and Debian-based distributions (Debian, Ubuntu). It also configures servers for **rootless** execution with the specified user, adapting their home directory to allow the creation and management of containers in service mode.

## Requirements

The **podman_user** variable must be defined and assigned to an existing non-root user on the system. This user must not be a system user and must have their default home directory, for example: **/home/username/**. The default repositories for the **Red Hat** and **Debian** distributions must be configured beforehand.

## Description of Variables

|Name|Type|Description|Default value|
|----|----|-----------|-------------|
`podman_user`|str|non-root user defined to run the **podman** processes|`""`
`podman_subid_range`|str|range of values ​​for an entry in /etc/subuid and /etc/subgid for the user **podman**|`"100000:65536"`
`podman_version`|str|**podman** version to install|`"latest"`

## Requirements

None.

## Examples Playbook

- Role installation

```bash
mkdir -p $HOME/install-podman
```

```bash
vim $HOME/install-podman/requirements.yml
```

```yaml
- name: ansible-role-podman-installer
  src: git+https://github.com/willbrid/ansible-role-podman-installer.git
  version: v0.0.5
```

```bash
cd $HOME/install-podman && ansible-galaxy install --force -r requirements.yml
```

> Note: It is assumed that a `hosts.ini` file (in the `$HOME/install-podman` directory) is defined, containing the inventory of `all` group servers, using `Debian` or `RedHat` distributions.

- Configuring a playbook and installing Podman

```bash
vim $HOME/install-podman/playbook.yml
```

```yaml
---
- hosts: all
  become: yes

  vars:
    podman_user: "vagrant"

  roles:
    - ansible-role-podman-installer
```

```bash
cd $HOME/install-podman && ansible-playbook -i hosts.ini playbook.yml
```

## License

MIT

## Author Information

William Bridge NGASSAM
