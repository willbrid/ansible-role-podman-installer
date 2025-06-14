# Ansible-role-podman-installer

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/willbrid/ansible-role-podman-installer/blob/main/LICENSE) [![CI](https://github.com/willbrid/ansible-role-podman-installer/actions/workflows/ci.yml/badge.svg)](https://github.com/willbrid/ansible-role-podman-installer/actions/workflows/ci.yml)

Le rôle **ansible-role-podman-installer** permet d'installer le gestionnaire de conteneurs **podman** pour les distributions basées sur RedHat (RHEL, CentOS, Rocky Linux) et Debian (Debian, Ubuntu). Il configure également les serveurs pour l'exécution en mode **rootless** avec l'utilisateur spécifié, en adaptant son répertoire personnel afin de permettre la création et la gestion des conteneurs en mode service.

## Exigences

La variable **podman_user** doit être définie et attribuée à un utilisateur non root existant sur le système. <br>
Cet utilisateur ne doit pas être un utilisateur système et doit avoir son répertoire home par défaut, exemple : **/home/username/**. <br>
Les référentiels par défaut des distributions **RedHat** et **Debian** doivent être préalablement configurés.

## Description des Variables

|Nom|Type|Description|Valeur par défaut|
|---|----|-----------|-----------------|
`podman_user`|string|utilisateur non root défini pour exécuter les processus **podman**|`""`
`podman_subid_range`|string|plage de valeurs d'une entrée /etc/subuid et /etc/subgid pour l'utilisateur **podman**|`"100000:65536"`
`podman_version`|string|version de **podman** à installer|`"latest"`

## Dépendances

Aucune.

## Exemples Playbook

- Installation du rôle

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

> Note: On suppose qu’un fichier `hosts.ini` (dans le repertoire `$HOME/install-podman`) est défini, contenant l’inventaire des serveurs de groupe `all`, utilisant des distributions `Debian` ou `RedHat`.

- Configuration d'un playbook et Installation de podman

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

## Licence

MIT

## Informations sur l'auteur

William Bridge NGASSAM
