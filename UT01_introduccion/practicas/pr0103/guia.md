# 1
Hago el archivo de configuracion
```ruby
Vagrant.configure("2") do |config|
 
  config.vm.box = "gusztavvargadr/ubuntu-server-2004-lts"
  config.vm.hostname = "web-rmg"

  config.vm.provider "virtualbox" do | vb |
    vb.name ="web_server"
    vb.memory = 3072
    vb.cpus = 2
  end
  
  config.vm.network "private_network" , ip: "172.16.0.0/16"
  config.vm.network "public_network" , ip: "10.99.0.0/16"

end
```
# 2 servidor Apache
## 2.1 Instalar un servidor Apache
```bash
- sudo apt-get install apache2
```
Si buscamos desde la máquina fisica localhoft:8080 nos saldra la pagina del
servidor apache

## 2.2 Creamos un html enlazado a otros dos archivos html

Lo creamos en la carpeta que hemos enlazado

Al buscar localhost:8080 nos saldrá nuestra pagina html

Sacar la captura de pantalla de la pagina web

![imagen](pagina%20html.png)

[Volver](../../index.md)