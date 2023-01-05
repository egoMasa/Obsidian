# Guide VRF (Virtual Routing and Forwarding)

# I) Généralités
* Ce sont des tables de routage virtuelle
* Permettent de créer des réseaux privés virtuels au sein d'un réseau physique (LAN)
* Permettent de créer plusieurs réseaux privés au sein d'un réseau étendu (WAN)
* Il permet de segmenter un routeur en plusieurs sous chemins, isoler par rapport aux autres
* Chaque VRF possède sa propre table de routage
	* Séparer les paquets en fonction de la destination
	* Maintenir privés et sécurisé

# II) Fonctionnement
* Chaque VRF possède un table de routage avec informations de routage pour les paquets à destination cette VRF
* Quand paquet entre dans le routeur, il est acheminé en fonction de sa destination grace à la table de routage de cette VRF

# III) Système de conjonction 

## Route dinstinguisher 
* Distinguer à quelle VRF appartient la route
* Identifiant unique qui distingue les routes appartenant à différentes VRF
* Empêche les conflits de routage entre différentes VRF et garantit que chaque route achemine bien vers la bonne VRF
```
ASN:nn
```
* On renseigne une RD par VRF sur chaque routeur

## Route target (A quelle VRF sera livré le paquet)
* Identifiant unique qui spécifie à quelle VRF une route doit être acheminé
* Pour configurer les règles de routage qui déterminent comment sont acheminés les paquets 
* BGP est utilisé pour acheminer les paquets entre différentes VRF avec les numéro ASN 
### 1) Route target import
* Spécifie les VRF auxquelles un routeur doit acheminer les paquets selon leur destination
* J'import ce que les autres exportent
### 2) Route target export
* Spécifie les VRF auxquelles un routeur doit envoyer les paquets selon leur source
* J'exporte ce que les autres importent

# 1) Création et assignation

* Création des VRF
```
ip vrf @nom_vrf
	rd @ASN:@Num_vlan
```
* Assignation VRF à une interface
```
int G0/0
	ip vrf forwarding @nom_vrf
	ip address
```
* CAS : Plusieurs VRF sur un lien 
```
int G0/0.10
	encapsulation dot1q 10
	ip vrf forwarding @nom_vrf
	ip address @ip @masque
int G0/0.20
	encapsulation dot1q 20
	ip vrf forwarding @nom_vrf
	ip address @ip @masque
```

# 2) Mise en place du routage

* Configurer OSPF pour chaque VRF
```
router ospf XX vrf @nom_vrf
	router-id X.X.X.X
	network @ip @masque_invert area X
```
* Redistribuer route statique par défaut
```
router ospf 10 vrf @nom_vrf
	redistribute default originate
```
* Vérification table de routage
```
sh ip route vrf @nom_vrf
```

# 3) Fuite de route MP-BGP

* BUT : Faire fuiter routes d'une VRF vers une autre
* MP-BGP : Transporter les routes d'une VRF entre des routeurs PE

## Etape 1 : Distinguer VRF via RD

* Distinguer VRFs
```
ip vrf @nom_vrf
	rd @ASN:@Num_vlan
```

## Etape 2 : Configurer MP-BGP (routage BGP)

* Création loopback (obligatoire en BGP)
```
int lo1
	ip address 1.1.1.1 255.255.255.255
	ip ospf network point-to-point
```
* Activer relation de voisinage pour transporter routes VPNv4
```
router bgp @ASN
	neighbor @ip remote-as @ASN
	neighbor @ip update-source Loopback1
	adress-family vpnv4
		neighbor @ip activate
```
* Spécifier famille d'adresses IPv4 pour chaque VRF et redistribuer préfixes OSPF VRF
```
router bgp @ASN
	address-familly ipv4 vrf @nom_vrf
		redistribute ospf @process_ospf vrf @nom_vrf route-map @nom_route
```
* Création ACL pour autoriser réseau VRF
```
access-list @num_acl permit @reseau @masque_invert
```
* xx
```
route-map @nom_route permit 10
	match ip address @num_acl
```
* Vérifications
```
sh ip bgp vpnv4 all
sh ip bgp neighbors | section remote
sh run | section bgp
```

## Etape 3 : Activer fuite des routes via RT

* Configuration
```
ip vrf @nom_vrf
	route-target export @ASN:XXX
	route-target import @ASN:XXY
```

* Vérifier importations/exportations ont bien fonctionné pour chaque famille d'adresses MP-BGP
```
sh ip bgp vpnv4 all
```

## Redistribution d'OSPF dans BGP
```
router bgp @ASN
	address-family ipv4 vrf @nom_vrf
		redistribute ospf 11 vrf @nom_vrf
```

## Redistribution de BGP dans OSPF
```
router ospf XX vrf @nom_vrf
	redistribute bgp @ASN subnets
```