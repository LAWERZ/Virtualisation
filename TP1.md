# Tp1

## Adresse mac des deux VM

```ip a```  

_Première machine_
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:01:3b:5c brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.1/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe01:3b5c/64 scope link
       valid_lft forever preferred_lft forever
```

_Deuxième machine_
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e2:5d:86 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.2/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee2:5d86/64 scope link
       valid_lft forever preferred_lft forever
```

MAC première machine : 08:00:27:01:3b:5c 
MAC deuxième machine : 08:00:27:e2:5d:86

## Passer les VM en ip statique

Création et édition du fichier ***ifcfg-enp0s3*** sur les deux VM

``` sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3```  

Fichier de la première machine
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.1.1.1
NETMASK=255.255.255.0
```

Fichier de la seconde machine
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.1.1.2
NETMASK=255.255.255.0
```
Ensuite 
```sudo nmcli con reload```  
```sudo nmcli con up enp0s3```  

Après un ```ip a``` sur la première machine:  
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:01:3b:5c brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.1/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe01:3b5c/64 scope link
       valid_lft forever preferred_lft forever
```
IP : **10.1.1.1/24**  

Après un ```ip a``` sur la seconde machine: 
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d1:f1:3d brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.2/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fed1:f13d/64 scope link
       valid_lft forever preferred_lft forever
```
IP : **10.1.1.2/24**  

## Effectuer un ping entre les deux machines  

1. ```ping``` de ```10.1.1.1``` vers ```10.1.1.2```  

```ping 10.1.1.2```  
```
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=0.672 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=0.589 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=0.767 ms
```

2. ```ping``` de ```10.1.1.2``` vers ```10.1.1.1```  

```ping 10.1.1.1```
```
PING 10.1.1.1 (10.1.1.1) 56(84) bytes of data.
64 bytes from 10.1.1.1: icmp_seq=1 ttl=64 time=0.653 ms
64 bytes from 10.1.1.1: icmp_seq=2 ttl=64 time=0.817 ms
64 bytes from 10.1.1.1: icmp_seq=3 ttl=64 time=0.418 ms
```
## Lancer Wireshark  

La requète utilisé lors d'un ping est ```IMCPv6```  


# II. Ajoutons un switch
