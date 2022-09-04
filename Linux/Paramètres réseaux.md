# Guide réseaux Linux
## I) Affichage paramètres principaux
* IP+Masque+MAC+Interfaces
```
(machine)root#ip a
```

* Passerelle par défaut
```
(machine)root#ip route
```

* DNS
```
(machine)root#cat /etc/resolv.conf
```

## II) Paquet complémentaire
```
(machine)root#apt-get install net-tools
```

## III) Interfaces réseaux
* Activer interface
```
(machine)root#ifup @interface
```

* Désactiver interface
```
(machine)root#ifdown @interface
```

## III) Configurer paramètres
### Modifier IP,NETMASK,GW dans ***/etc/network/interfaces***
```
(machine)root#sudo nano /etc/network/interfaces
```
* Remplir avec les lignes suivantes 
```
allow-hotplug @interface
	iface @interface inet static
		address @ip
		netmask @masque
		gateway @ip_saut
```

### Modifier DNS dans ***/etc/resolv.conf***
```
(machine)root#sudo nano /etc/resolv.conf
```
* Remplir avec les lignes suivantes 
```
nameserver @ip_dns
```

### Modifier DHCP
* Installer paquet ***dnsmasq*** 
```
(machine)root#apt-get install dnsmasq
```
* Activer DHCP coté serveur dans ***/prc/sys/net/ipv4/ip_forward
```
1
```
 * Créer fichier ***/etc/dnsmasq.d/X.X.X.conf***
```
dhcp-range = @ip_debut,@ip_fin,@masque,@XXh
dhcp-range = @MAC,@nom,@ip_attribué,@XXm
```
* Activer DHCP coté client dans ***/etc/network/interfaces
```
auto @interface
iface @interface inet dhcp
```
