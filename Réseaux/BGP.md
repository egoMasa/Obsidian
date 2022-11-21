# 1) Généralités

* BGP = ***Border Gateway Protocol***
* ***AS*** = Système autonome avec la même politique de routage
* ***ASN*** = Identifiant d'un AS
	* ***IANA*** : 
		* 16 bits
		* ***Réservé*** : 64496-64534
		* ***Privés*** : 64512-65534
* Routage de l’Internet global
* Routage pour nombre de réseau XXL 
* Ne jamais redistribuer de routes BGP à un autre protocole LAN (explosion)
* Route statique par défaut équivaut à BGP maintenant
* IGP vise la performance
* BGP vise le respect des politiques, pas forcement la plus rapide maintenant, mais celle qui arrange le plus selon nos critères, contrôle du trafic quand il sort du réseau
* Remplacement de ***EGP***
* Policy Base Routing Protocol : process whereby the device puts packets through a route map before routing them
* Protocole normalisé de ***routage externe*** (dehors du LAN) et qui relie plusieurs ***système autonome*** (LAN avec protocole routage interne comme OSPF,RIP,EIGRP et qui peuvent communiquer entre eux)
* Adjacence = Relation de peering = BGP Neighbor
* Il peut y avoir un autre protocole présent (routeur) entre deux adjacence entre routeur BGP

# 2) Principes
* Internet est un ensemble de systèmes autonomes (LAN) interconnectés 
* BGP est celui qui va relier les différents systèmes autonomes entre eux
* BGP est un protocole à vecteur de chemin : définit un trajet a partir d’une destination et les caractéristiques du PATH qui mène à cette destination.
	* Empêche boucle de routage, prend un chemin qu'il connait déjà
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
* Activer protocole BGP 
```
R1(config)#router bgp @ASN
```
* Annoncer son réseau
```
R1(config-rtr)#network @mon_reseau mask @masque
```
* Annoncer ses voisins (ASN)
```
R1(config-rtr)#neighbor @IP_ASN_VOISIN remote-as @ASN_voisin
```
* Modifier router-id BGP
```
R1(config-rtr)#bgp router-id X.X.X.X
```
* Exemple de configuration
```
R1(config)#router bgp 65329
R1(config-rtr)#bgp router-id 1.1.1.1
R1(config-rtr)#network 192.168.1.0 mask 255.255.255.0
R1(config-rtr)#neighbor 10.0.0.0 remote-as 65330
```