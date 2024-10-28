# PR012 Entornos Multimáquina
## Vagrantfile

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.network "private_network" , ip: "192.168.0.0", netmask: "255.255.255.0"
  config.vm.define "win19server" do |serverwindows|
    serverwindows.vm.box = "gusztavvargadr/windows-server-2019-standard"
    serverwindows.vm.hostname = "server-rmg"
    serverwindows.vm.network "private_network" , ip: "192.68.0.10"
    serverwindows.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
      vb.cpus = 4
    end
  end
  config.vm.define "win10user" do |windowsusuario|
    windowsusuario.vm.box = "gusztavvargadr/windows-10"
    windowsusuario.vm.hostname = "w10-rmg"
    windowsusuario.vm.network "private_network" , ip: "192.68.0.20"
    windowsusuario.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
      vb.cpus = 2
    end
  end
end
```

## Comprobación del funcionamiento de la máquina
Nos conectamos con 
```
vagrant rdp win19server
vagrant rdp win10user
```

[Volver](../../index.md)