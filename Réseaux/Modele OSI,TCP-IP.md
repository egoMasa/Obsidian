# Guide OSI/TCP 

## I) Modèle OSI et TCP/IP

* Afin de bien comprendre comment nos systèmes communiquent entre eux, on a défini plusieurs modèles qui se ressemblent pour mettre chaque système de communication sur la même logique de connexion et de transport d’information.

![[Pasted image 20220831223202.png]]

![[Pasted image 20220831223121.png]]

## II) Role de chaques couches

* 7) ***Application*** = Fournit la ***sémentique/sens*** (mail,HTTP) des échanges entre systèmes répartis

* 6) ***Presentation*** = Gère la ***representation*** de données, ***conversion*** de syntaxe, ***compression*** et ***décryptage***

* 5) ***Session*** = Structure les échanges avec de ***points de synchronisation*** (sauvegarde de l'état, % transfert)

* 4) ***Transport*** = Transfert de données appelés ***segment***, de ***taille quelquonque***, sans perte, sans erreurs

* 3) ***Network*** = ***Acheminement*** de blocs de données de taille limités appelés ***paquets*** entre différents ***systèmes adjacent ou non***

* 2) ***Data Link*** = ***Transfert*** de blocs de bits structurés sous forme de ***trames*** et règles les ***méthodes d'accès au lien*** (CSMA/CD)

* 1) ***Physical***= Transfert de ***bits*** entre ***systèmes adjacents*** relié par un ***medium de communication*** (filère ou non filère)

## III) Encapsulation et décapsulation

* Lors d’une communication, chaque donnée suit une route identique où l’on va ajouter différentes informations de chaque couche dans l’autre afin que les données arrivent à destination et soit lisible.

* Chaque paquet IP contient l’IP source et l’IP destination  Les routeurs acheminent donc les paquets vers leur destination, de proche en proche.

![[Pasted image 20220906215158.png]]

![[Pasted image 20220906215308.png]]
![[Pasted image 20220906215639.png]]

## IV) Encapsulation et décapsulation

* Lors d’une communication, chaque donnée suit une route identique où l’on va ajouter différentes informations de chaque couche dans l’autre afin que les données arrivent à destination et soit lisible.