# OSPF (gros réseaux)

### Fonctionnement :

* OSPF fonctionne avec un système de zone afin d’alléger les performances. Un routeur ne connaît que les informations de sa propre zone OSPF, il ne pourra communiquer qu’avec ceux présent dans la même zone. ***L’air BackBone (0)*** joue le rôle de diffuser les informations entre les différentes zones OSPF. 

* L’un des atouts de OSPF est qu’il permet d’ajouter des routes/réseaux apprises via d’autres protocoles comme RIP par exemple ce qui le rend flexible et surtout extensible très facilement. A noter que les adresses MULTICAST ***224.0.0.5*** et ***224.0.0.6*** sont réservées à pour l’envoyer des mises à jour de routage.

* A propos de son fonctionnement, il utilise le paquet “Hello” qui est un message permettant de découvrir les voisins, tenir à jour la table de routage. Ce paquet est envoyé toutes les 10s et si un voisin ne renvoie aucune réponses il est considéré comme DOWN pendant 40s

* Il existe un processus d’élection de deux routeurs dans chaque zone jouant le rôle de délégué et sous délégué qui va la table de routage qui sera transmise à tous les routeurs de la zone. ***Le délégué (DR)*** est le routeur avec la priorité la plus élevée (ip ospf priority), sinon l’ID du routeur le plus élevé (router ID), sinon la loopback avec l’IP la plus haute, sinon l’interface réseau la plus haute.

### Mises à jour:
* Concernant les mises à jour, elles sont transmises uniquement en MULTICAST comme ceci

	-   Les mises à jour envoyées par le DR utilisent l’adresse IP ***224.0.0.5***
    
	-   Les mises à jour envoyées au DR et au BDR utilisent l’adresse IP ***224.0.0.6***

### Commandes OSPF :

-   Activer OSPF sur le routeur
```
R1(config)#router ospf @ID
```

-   Annoncer un réseau voisin
```
R1(config-router)#network @ip_reseau_voisin @masque_inverse area @area`
```

-   Changer priorité de l’interface routeur
```
R1(config-router)#ip ospf priority <0-255>
```

-   Changer ID du router
```
R1(config-router)#router-id @IP
```

-   Partager route par statique/defaut
```
R1(config-router)#default-information originate
```

-   Empêcher interface de partager mise à jour de la table
```
R1(config-router)#passive-interface @interface
```

## Multizone
* On effectue un routage précis selon une zone délimitée afin de limiter les changement de topologies
* ***LSA***  = Link State Advertisement : Maintenir topologie commune : Envoie de MAJ
	* 1,2 dans une zone
	* 3
* ***SPF*** = Shortest Path First : Algorithme, on cherche à reduire sa fréquence de calcul
* ***LSDB*** : Link State Data Base
* ***ABR*** = Area Border Router : Router entre 2 zones dont la BackBone
* ***ASBR*** = Autonomous System Border Router : Avec BGP
* Réduire table de routage
* ***LSU*** = Link State Update
* ***LSR*** = Link State Request
* ***IA*** = Inside Area
* On doit forcement garder une zone backbone 0 avec les routeurs les plus puissants car ils ont accès à toutes les tables de chaque zones (centralise)
* Exemple de configuration
```
router ospf 1
	router-id X.X.X.X
	network @ip_voisin @masque_voisin_invert area @area_ip_voisin
```
* Résumé de route (à l'intérieur du processus, en multizone seulement)
```
router ospf 1
	area X range @route_resumée
```

# IPV6
* Echange entre routeurs via ***link-local*** 
* Multicast OSPF routers : ***FF02::5***
* Multicast Designated routers : ***FF02::6***
* Configuration
```cisco
ipv6 unicast routing

ipv6 router ospf 1
	router-id 1.1.1.1
	
int G0/0/0
	ipv6 address 2001:db8:cafe::1/64
	ipv6 ospf 1 area 0
	ipv6 ospf priority 20
	ipv6 ospf cost 20

	
sh ipv6 route ospf
sh ipv6 ospf neighbor
sh ipv6 int brief
```
* Multizone OSPFv3
```
ipv6 unicast-routing
ipv6 router ospf 1
	router-id X.X.X.X
int G0/0
	ipv6 ospf 1 area @area_reseau_voisin_interface
```
* Commande de vérification
```
show ipv6 ospf
show ipv6 route
show ipv6 ospf database
show ipv6 ospf interface
show ipv6 ospf neighbor
```
* Résumé de route (à l'intérieur du processus, en multizone seulement)
```
ipv6 router ospf 
	area X range @route_resumée
```