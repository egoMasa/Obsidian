# Guide Adressage IPV6

## I) Principe
### 0) Généralités
* Intérêt IPV6 = Répondre à la pénurie d'adresses IPV4
* Sur 16 octets de 8bits donc 128 bits au total 
```
A524:72D3:2C80:DD02:0029:EC7A:002B:EA73 = 8 champs
 16   16   16   16   16   16   16   16  = 8 champs de 16bits,donc une moitié de champs vaut 8bits
```
* Une IPv6 est divisé entre réseau et identifiant
```
FE80::0234:56FF:FE78:91BC/64
Identifiant réseau = FE80::0000:0000:0000:0000
Identifiant interface = 0234:56FF:FE78:91BC
```
* IPV6 est plug and play, il peut se configurer automatiquement via le protocole ***SLAAC***
* Plus forcement besoins du NAT/PAT, on connexion peut se faire de bout en bout
* Système de sécurité grace à IPsec
* En tête du paquet sans diffusion, somme de controle
* Le broadcast n'existe pas en IPV6
* ARP est remplacé par protocole NDP (Neighbor Discovery Protocol)
	* Résolution d'adresse MAC
	* Configuration automatique
	* Message en ICMPv6
### 1) Protocoles
* SLAAC = Fournit IP, produit table de routage NDP
* RIP = ?
### 2) Types d'adresses (lien,local,globale,multicast)
* ***Unicast*** = A une interface unique (un vers un)
* ***Multicast*** = A plusieurs interface (un vers plusieurs)
* ***Anycast*** = Au plus proche d'un groupe qui le transmet lui après(routeur) (un vers le plus proche)
* Les hosts sous IPV6 possèdent 4 adresses IPV6
	* 1 - ***Lien local unicast*** (link local) = Sur un cable uniquement : ***FE80::/10*** non routable
	* 2 - ***Unique local*** (ULA)= Dans le réseau LAN privé uniquement , routable dans le LAN en ***FC00::/7***
	* 3 - ***Globale unicast*** = Dans le réseau publique WAN, routable sur internet en ***2000::/3***
		* Plusieurs niveau d'encapsulation de réseau via le masque (faible = générale)
		* /3 = IANA
		* /23 = RIR
		* /32 = LIR
		* /48 = Sites
		* /64 = Sous réseau
	* 4 - ***Multicast*** = ***FF00::/8***

### 3) Adresses réservées
* Route par défaut = ***::/0*** équivalent de ***0.0.0.0/0***
* Non spécifié = ***::/128*** 
* Loopback = ***::1/128*** équivalent de ***127.0.0.1***
* Lien local unicast = ***FE80::/10***
* Multicast = ***FF00::/8***
* Globale unicast : Les autres
### 4) Raccourcir IPV6
* On peut barrer une ligne de zero 1 fois seulement
* Si autre champs de zero il deviennent 1 zero pour chaque
* On peut barrer un zero si il est au début
```
2031:0000:130F:0000:0000:09C0:876A:130B
2031:0:130F::9C0:876A:130B
```

### 5) Adressage et attribution
#### 5.1)Manuelle via format IEEE (EUI-64)
* On prend l'adresse MAC (48bits, 6octets)
* On prend le premier octets (2 premier chiffre)
* On passe de 0 à 1 le second bits de droite
* On rajoute 2 octets de bourrage FF,FE entre le 3ème octet et le 4ème
```
00:21:86:10:CE:84
02:21:86:FF:FE:10:CE:84	
```
#### 5.2)Automatique via SLAAC/DHCPv6
* Activer transmission IPv6
```
R(config)#ipv6 unicast-routing
```
* Activer IPv6
```
R(config)#int G0/0/0
R(config-if)#ipv6 enable
```
* Attribuer adresse IPv6 à une interface
```
R(config)#int @interface
R(config-if)#ipv6 address PREFIX/MASQUE
```
* Attribuer adresse lien local à une interface
```
R1(config)#interface @interface
R1(config-if)#ipv6 address FE80::X link-local
```

