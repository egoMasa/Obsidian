# Guide réseaux Linux
## 1) Paramètres réseaux
* Afficher IP,Masque,MAC,Interfaces
```
(machine)root#ip a
```

* Passerelle par défaut
```
(machine)root#ip route
```

* Afficher table de routage
```
netstat -nr
route -n
ip route
```
* DNS
```
(machine)root#cat /etc/resolv.conf
```

## 2) Paquet net-tools
```
(machine)root#apt-get install net-tools
```

## 3) Manipulation interfaces 

* Lister nom interfaces réseaux
```
ls /sys/class/net
```
* Activer interface
```
(machine)root#ifup @interface
```

* Désactiver interface
```
(machine)root#ifdown @interface
```

## 4) Configurer interfaces
* Ajouter une interface réseau ipv4 automatique
```
auto @interface
allow-hotplug @interface
iface @interface inet dhcp

ou

auto @interface
allow-hotplug @interface
iface @interface inet auto
```
* Ajouter une interface réseau ipv6 automatique
```
auto @interface
allow-hotplug @interface
iface @interface inet6 dhcp

ou

auto @interface
allow-hotplug @interface
iface @interface inet6 auto
```
* Ajouter une interface réseau ipv4 static
```
auto @interface
iface @interface inet static
	address @ip_interface
	gateway @ip_gateway
```
* Ajouter une interface réseau ipv6 static
```
auto @interface
iface @interface inet6 static
	address @ipv6_interface
	gateway @ipv6_gateway
```

## 5) DNS
* Editez fichier ***/etc/resolv.conf***
```
nameserver @ip_dns
```


## 6) DHCP
* Installer paquet ***dnsmasq*** 
```
(machine)root#apt-get install dnsmasq
```
* Activer DHCP au demarrage
```
systemctl enable dnsmasq
```
* Editez fichier ***/prc/sys/net/ipv4/ip_forward
```
1
```
 * Créer fichier ***/etc/dnsmasq.d***
```
log-dhcp #logs dans /var/log/messages
dhcp-range = @ip_debut,@ip_fin,@masque,@XXh
dhcp-host = @MAC,@nom,@ip_attribué,@XXm
```
* Editez ***/etc/network/interfaces*** (cote client)
```
auto @interface
iface @interface inet dhcp
```
