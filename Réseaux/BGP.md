# 1) Généralités

* BGP = ***Border Gateway Protocol***
* AS = Système autonome avec la même politique de routage
* ASN = Identifiant entier de 32 bits d'un AS
* Routage de l’Internet global
* Version XXXXX...XL
* Remplacement de EGP
* Policy Base Routing Protocol 
* Protocole de routage externe (dehors du LAN) et qui relie plusieurs système autonome (LAN avec protocole routage interne comme OSPF,RIP,EIGRP)

# 2) Principes
* Internet est un ensemble de sytèmes autonomes (LAN) interconnectés 
* BGP est celui qui va relier les différents systèmes autonomes entre eux
* BGP est un protocole à vecteur de distance : définit un trajet a partir d’une destination et les caractéristiques du PATH qui mène à cette destination.
* BGP est un protocole qui utilise TCP
* BGP version 4 = Dernière version qui supporte le VLSM
* BGP-MP (MutliProtocol) = Supporte IPV6

# Comparaison entre IGP et BGP
* IGP = Décisions de routage sur la métrique de meilleur chemin.
* BGP 
	* Policy Based Routing Protocol
	* Contrôle du routage en utilisant des attributs multiples (BGP attributes)
	* AS-PATH = Liste des BGP ASN que le routeur doit utiliser pour atteindre le réseau de destination
	* Optimiser la bande passante disponible en manipulant les attributs BGP
* On utilise BGP quand :
	* L'AS dispose de connexions multiples sur d'autres AS
	* L’AS autorise le trafic de transit pour atteindre d’autres AS
* On n'utilise pas BGP quand :
	* Connexion unique vers l'Internet où vers un autre AS
	* Manque de puissance CPU et RAM
	* Pas de compréhension du processus BGP et comment il gère les routes et les stratégies de PBR (Policy Based Routing)
* Il faut mieux utiliser des routes statiques ou routes statiques par défaut

# 3) Configuration 

* Entrer dans le mode configuration du BGP
```
router bgp autonomous-system
```
* Activer session BGP avec voisins
```
neighbor {ip-address | peer-groupe-name} remote-as autonomous-system
```
* Propager routes BGP
```
network {network-number} mask {network-mask}
```
* Exemple 
```
router bgp @num
	network {mon_network}
	neighbor {ip_saut} remote-as {num_ip_saut}
	
show ip bgp neighbors
show ip bgp summary
```
* Activer BGP en mode IPV6 (BGP-MP)
```
no bgp default ipv4-unicast
```
* Etablir phase de peering
```
neighbor <@IPv6> remote-as
```
* Configurer adresse de famille
```
address-family ipv6
```
* Activer peers voisins
```
neighbor <@IPv6> activate
```
* Annoncer des routes 
```
network {network-number/XX}
```
* Terminaison de l’adresse family
```
exit-address-family
```
* Exemple 
```
router bgp 1 
	bgp router-id 1.1.1.1 
	no bgp default ipv4-unicast 
	bgp log-neighbor-changes 
	neighbor 2010:AB8:0:2::2 remote-as 2 ! 
	address-family ipv6 
		neighbor 2010:AB8:0:2::2 activate 
		network 2010:AB8:2::/48 
		network 2010:AB8:3::/48 
		exit-address-family
		
show ip bgp neighbors
show bgp ipv6 unicast summary
```