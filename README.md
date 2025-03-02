Ansible-role-podman-installer
=========

Ce rôle Ansible installe le gestionnaire de conteneurs **podman** pour les distributions basées sur RedHat (RHEL, CentOS, Rocky Linux) et Debian (Debian, Ubuntu). 

Exigences
------------

La variable **podman_user** doit être définie et attribuée à un utilisateur non root existant sur le système.

Description des Variables
--------------

|Nom|Type|Description|Valeur par défaut|
|---|----|-----------|-----------------|
`podman_user`|string|utilisateur non root défini pour exécuter les processus **podman**|`""`

Dépendances
------------

Aucune.

Exemples Playbook
----------------

- Installation du rôle et définition du fichier d'inventaires

```
mkdir -p $HOME/install-podman/roles
```

```
vim $HOME/install-podman/requirements.yml
```

```
- name: ansible-role-podman-installer
  src: https://github.com/willbrid/ansible-role-podman-installer.git
  version: main
```

```
cd $HOME/install-podman && ansible-galaxy install -r requirements.yml --roles-path roles
```

```
vim $HOME/install-podman/hosts.ini
```

```
[podman]
192.168.56.6
192.168.56.7
```

NB: Les adresses IP définies dans le fichier **$HOME/install-podman/hosts.ini** sont fournies à titre d'exemple et doivent être remplacées par les vôtres.

- Configuration d'un playbook et Installation de podman

```
vim $HOME/install-podman/playbook.yml
```

```
---
- hosts: podman
  become: yes

  vars:
  - podman_user: "vagrant"

  roles:
  - ansible-role-podman-installer
```

```
cd $HOME/install-podman && ansible-playbook -i hosts.ini playbook.yml
```

Licence
-------

BSD,MIT

Informations sur l'auteur
------------------

William Bridge NGASSAM