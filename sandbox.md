# Sandbox avec virtualbox 7.0 et vagrant

```
mkdir $HOME/install-podman && cd $HOME/install-podman
```

```
wget https://download.virtualbox.org/virtualbox/7.0.20/VBoxGuestAdditions_7.0.20.iso
```

```
vim Vagrantfile
```

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
dnf remove -y podman runc
SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true
  config.vbguest.iso_path = "./VBoxGuestAdditions_7.0.20.iso"

  # General Vagrant VM configuration
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
    v.linked_clone = true
  end
  
  # Rocky Server
  config.vm.define "rocky-server" do |srv|
    srv.vm.box = "bento/rockylinux-8"
    srv.vm.box_version = "202502.21.0"
    srv.vm.hostname = "rocky-server"
    srv.vm.network :private_network, ip: "192.168.56.6"
    srv.trigger.after :up do |trigger|
      trigger.run_remote = { inline: $script, privileged: true }
    end
  end

  # Ubuntu Server 22.04
  config.vm.define "ubuntu-server" do |us|
    us.vm.box = "bento/ubuntu-22.04"
    us.vm.box_version = "202407.23.0"
    us.vm.hostname = "ubuntu-server"
    us.vm.network :private_network, ip: "192.168.56.7"
  end

  # Ubuntu Server 24.04
  config.vm.define "ubuntu-server2" do |us|
    us.vm.box = "alvistack/ubuntu-24.04"
    us.vm.box_version = "20250301.0.0"
    us.vm.hostname = "ubuntu-server2"
    us.vm.network :private_network, ip: "192.168.56.8"
  end
end
```

```
vagrant up
```