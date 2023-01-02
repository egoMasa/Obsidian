# Guide Routage dynamique

## I) Principe et explication

* Quand on vient à travailler sur des réseaux complexes par leur taille et nombre d’équipement réseau, il est préférable de passer sur un système de routage dynamique. Son principe consiste à ***rendre autonome les routeurs dans leur choix de route*** (plus besoins de route statique). Pour cela chaque routeur annonce le/les réseaux auxquels ils sont directement connectés et transmet leur réseau connu à leur voisins routeurs. De ce fait, un plan du réseau se fait automatiquement et ***chaque routeur connaît la même topologie du réseau***.

* De nombreux protocoles de routage dynamique existent, avec chacun un mode de décision de quelle route est jugée plus performante/préférable pour arriver à destination. Pour cela des paramètres précis sont décrits et utilisés 

* ***Convergence*** = Correspond au temps que met l'ensemble des routeurs d'une topologie afin de mettre à jour l'ensemble des tables de routages

* ***Répartition de charge*** = Capacité d'un routeur à supporter plusieurs chemins à couts egaux vers une destination, ce qui fait que un paquets peut aller vers une destination via plusieurs interfaces

* La ***distance administrative*** = Définit la fiabilité d'un protocole de routage, chaque protocole en a un différent. Plus son numéro est bas, plus la route est considérée fiable. C’est aussi un moyen de savoir par quel protocole est apprise une route.
![[Pasted image 20220831224843.png]]

-   La ***métrique*** = Note selon le protocole de routage qui détermine le meilleur itinéraire vers la destination, chaque protocole utilisent des caractéristiques différents, Métrique de 16 égal à valeur infinie
![[Pasted image 20220831224925.png]]

* Un des soucis est que certains protocoles gagnent le choix de la route alors qu'ils ne sont pas les plus rapides en réalité. Par exemple OSPF gagne contre RIP même si le nombre de sauts IP est plus avantageux que le coût en bande passante. C’est pour cela que la ***distance administrative peut être modifiée*** afin de gérer efficacement notre réseau.

* La route par défaut quand elle est toujours utilisé afin de pouvoir continuer à ***rejoindre des réseaux non présents dans la table de routage***

# II) Routage intérieur (IGP)

## 1) Vecteur de distance
* Les protocoles à vecteur de distance ***additionne les distances*** pour trouver la meilleure route
* ***Nombres de sauts (hop)***
* Les routeurs se partage ***entièrement leur table de routage*** à leurs voisins
* Concu pour des réseaux plats
* Convergence lente
* Mise à jour régulière en Broadcast/Multicast
* ***EIGRP***  
	* 224.0.0.10
* ***RIP***,***RIGnG***
	* V1 : UDP520 - `255.255.255.255`
	* V2 : UDP520 - `224.0.0.9`

## 2) Etat de lien
* Les protocoles à état de lien ***calcul le cout de chaque route*** afin de prendre ***la meilleure***
* ***Chemin le plus court*** (rapidité du chemin mais si chemin avec plus de sauts)
* Cout de la bande passante au total par exemple
* Les routeurs collectent l'ensemble des ***couts de chaque lien*** et les meilleures sont intégrés à la ***table de routage commune***, on parle de partage de liaisons
* Convergence rapide
* Repartition de charge
* Mises à jour immédiate à chaque changement de topologie/modifications
* ***OSPFv2/v3***
	* `224.0.0.5`
	* `224.0.0.6`
	* `FF02::5`
	* `FF02::6`
* ***IS-IS***
* Ils maintiennent des ***relations*** avec les routeurs voisins