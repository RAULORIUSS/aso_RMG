# 1 Preparación de la máquina y configuración de la red
## 1.2
vagrant@ubuntu2204:~$ sudo nano /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    eth0:
      dhcp4: true
      dhcp6: false
    eth1:
      dhcp4: true

      vagrant@ubuntu2204:~$ sudo netplan apply

      vagrant@ubuntu2204:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:8c:69:41 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    inet 10.0.2.15/24 metric 100 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 86397sec preferred_lft 86397sec
    inet6 fe80::a00:27ff:fe8c:6941/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:6d:70:c3 brd ff:ff:ff:ff:ff:ff
    altname enp0s8
    inet 192.168.56.101/24 metric 100 brd 192.168.56.255 scope global dynamic eth1
       valid_lft 597sec preferred_lft 597sec
    inet6 fe80::a00:27ff:fe6d:70c3/64 scope link
       valid_lft forever preferred_lft forever

## 1.3
haciendo un ping en mi pc compruebo que hay conectividad

