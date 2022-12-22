# Guide ISIS

# I) C'est quoi IS-IS ?
* IS-IS : ***Intermediate system to intermediate system***
* Il s'agit d'un protocole de ***routage interne*** (IGP) 
	* ***Multi-protocoles***
	* ***Etats de lien*** : Calculer le meilleur chemin pour joindre une destination à partir des métriques (cost) définis sur les liens
* Utilisé à l'intérieur d'un ***AS*** (Autonomous System)
* Tous les routeurs IS-IS maintiennent une ***vue topologique commune*** qui est ***construite individuellement*** et ensuite ***partagé entre tous les routeurs IS-IS***
* Les paquets sont transmis par le ***plus court chemin*** 
* Même algorithme de calcul de route que OSPF
* Utilise le ***multicast*** pour ***découvrir les routeurs voisins*** avec ***paquet "hello"*** 
* Permet l'***authentification des MAJ***
* ***Système de zones ***
	* ***Level 1 ***
		* Dans ***une aire***
		* Routeurs communiquent ***qu'avec des routeurs level 1***
	* ***Level 2***
		* ***Entre aire***
		* Routeurs communiquent ***qu'avec des routeurs level 2***
	* ***Level 1-2***
		* ***Les deux***
		* Routeurs communiquent autant ***avec des routeurs level 1 et 2***
* ***Chaque routeur IS-IS*** fait partie d'***une seule aire uniquement*** 
* Pas de notion de ***zone backbone 0***
* ***NET*** : 
	* ***Adresse attribuée*** à une ***instance*** du protocole IS-IS
	* Comprend une ***adresse de zone***, un ***identifiant de système*** et un ***sélecteur N***
	* Seule la ***partie adresse de zone*** du NET peut différer si plusieurs NET sont attribués à une instance
* Construction individuelle de chaque routeurs d'une représentation topologique du réseau qui indique les sous-réseaux auxquels les routeurs IS-IS peuvent accéder ainsi que le chemin de moindre cout vers chacun des sous-réseau

# II) Comment on le configure ?
* Activer protocole ISIS
```
router isis @area-tag
```
* Configurer le NET 
```
net @network-entity-title
```
* Activer ISIS sur interface
```
int G0/0
	ip router isis
```
* Vérifications
```
show isis database
show isis topology
show ip route
show isis spf-log
```