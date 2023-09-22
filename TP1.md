# Tp1

## Adresse mac des deux VM

```ip a```  

_Première machine_
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:33:9a:8b brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.1/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe33:9a8b/64 scope link
       valid_lft forever preferred_lft forever
```

_Deuxième machine_
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d1:f1:3d brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.2/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fed1:f13d/64 scope link
       valid_lft forever preferred_lft forever
```

MAC première machine : 08:00:27:33:9a:8b 
MAC deuxième machine : 08:00:27:d1:f1:3d

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

Effectuer les commandes  
```sudo nmcli con reload```  
```sudo nmcli con up enp0s3```  

Après un **ip a** sur la première VM:  
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:33:9a:8b brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.1/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe33:9a8b/64 scope link
       valid_lft forever preferred_lft forever
```
Son ip est **10.1.1.1/24**  

Après un **ip a** sur la seconde VM: 
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d1:f1:3d brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.2/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fed1:f13d/64 scope link
       valid_lft forever preferred_lft forever
```
Son ip est **10.1.1.2/24**  

## Effectuer un ping entre les deux machines  

1. ```ping``` de ```10.1.1.1``` vesr ```10.1.1.2```  

```ping 10.1.1.2```  
```
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=1.36 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=1.37 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=1.64 ms
```

2. ```ping``` de ```10.1.1.2``` vesr ```10.1.1.1```  

```ping 10.1.1.1```
```
PING 10.1.1.1 (10.1.1.1) 56(84) bytes of data.
64 bytes from 10.1.1.1: icmp_seq=1 ttl=64 time=1.14 ms
64 bytes from 10.1.1.1: icmp_seq=2 ttl=64 time=1.18 ms
64 bytes from 10.1.1.1: icmp_seq=3 ttl=64 time=1.47 ms
```
## Lancer Wireshark  

La requète utilisé lors d'un ping est ```IMCP```  

### Bonus  
Utilisation de la commande ```ip neigh show```  

```
192.168.56.1 dev enp0s8 lladdr 0a:00:27:00:00:35 REACHABLE
192.168.56.103 dev enp0s8 lladdr 08:00:27:e9:69:95 STALE
```

# II. Ajoutons un switch
