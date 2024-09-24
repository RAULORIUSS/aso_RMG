Abro el visual estudio y me ubico en la carpeta de la practica 

Descargo la maquina virtual  

Inicializo la mquina que descargue y  hago el vagrant file 

Configuro el vagrant file como se me pide 

```ruby
Vagrant.configure("2") do |config|
 
  config.vm.box = "ubuntu-server"
  config.vm.hostname = "server-rmg"
    vp.memory = 2048
    vp.cpus = 2
  config.vm.synced_folder "./datazo", "/data"

end
```