# EIGRP (gros réseaux)

### Fonctionnement :

* EIGRP est un ***protocole sans classe*** à ***vecteurs de distance*** mais aussi à ***état de liens***.
* Il offre une ***convergence rapide***
* Il support ***VLSM*** 
* Utilise ***RTP*** pour l'envoi de ***messages EIGRP***
* Utilise l'algorithme ***DUAL*** pour determiner un ***chemin sans boucle***

* Trois modes d'envoi de mises à jours : 
	* limitées = MAJ only vers routeurs concernés
	* partielles = MAJ avec changement uniquement(pas toute la table)
	* non périodique = MAJ lors de changement de topologie

* EIGRP gère ***trois tables*** :
	* 1.  ***Neighbor Table*** : une table de voisinage est utilisée pour une livraison fiable des messages.
	* 2.  ***Topology Table*** : une table topologique qui contient toutes les routes EIGRP sans boucles.
	* 3.  ***Table de routage*** : pouvant contenir les meilleures routes EIGRP.

### Commandes EIGRP :

-   Activer OSPF sur le routeur
```bash
R1(config)#router eigrp 1
```
-   Rendre interface passive 
```bash
R1(config-router)#passive interface @interface
```
-   Modifier ID eigrp du routeur
```bash
R1(config-router)#eigrp router-id @id
```
-   Annoncer réseau voisin
```bash
R1(config-router)#network @ip_reseau @masque_invert
```
-   Vérifier configuration IPv4
```bash
R1#show ip protocols | begin eigrp
```
* Configurer delai "Hello"
```bash
R1(config-if)#ip hello-interval eigrp @instance @secondes
```
* Configurer delai "Hold-time"
```bash
R1(config-if)#ip hold-time eigrp @instance @secondes
```
* Modifier métrique interface 
```bash
R1(config-if)#bandwidth @num
```
* Résume de route
```
R1(config)# int G0/0/0
R1(config-if)#ip summary-address ...
```

# IPV6

* Adjacences via ***link-local*** 
* Groupe multicast : ***FF02::A***
* Shutdown de base, il faut l'activer
* Configuration
```cisco
ipv6 unicast routing
ipv6 router eigrp 1
	eigrp router-id 1.1.1.1
	no shutdown
	
int @int
	ipv6 adress 2001:db8:cafe::1/64
	ipv6 eigrp 1
	ipv6 summary-address eigrp ASN PREFIX/MASQUE
	
sh ipv6 route eigrp
sh ipv6 eigrp neighbor
sh ipv6 int brief

```