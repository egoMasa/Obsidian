# Guide Routage dynamique

## I) Principe et explication

* Quand on vient à travailler sur des réseaux complexes par leur taille et nombre d’équipement réseau, il est préférable de passer sur un système de routage dynamique. Son principe consiste à ***rendre autonome les routeurs dans leur choix de route*** (plus besoins de route statique). Pour cela chaque routeur annonce le/les réseaux auxquels ils sont directement connectés et transmet leur réseau connu à leur voisins routeurs. De ce fait, un plan du réseau se fait automatiquement et ***chaque routeur connaît la même topologie du réseau***.

* De nombreux protocoles de routage dynamique existent, avec chacun un mode de décision de quelle route est jugée plus performante/préférable pour arriver à destination. Pour cela des paramètres précis sont décrits et utilisés 

* La ***distance administrative*** = Définit la fiabilité d'un protocole de routage, chaque protocole en a un différent. Plus son numéro est bas, plus la route est considérée fiable. C’est aussi un moyen de savoir par quel protocole est apprise une route.
![[Pasted image 20220831224843.png]]

-   La ***métrique*** = Note selon le protocole de routage qui détermine le meilleur itinéraire vers la destination, chaque protocole utilisent des caractéristiques différents, Métrique de 16 égal à valeur infinie
![[Pasted image 20220831224925.png]]

* Un des soucis est que certains protocoles gagnent le choix de la route alors qu'ils ne sont pas les plus rapides en réalité. Par exemple OSPF gagne contre RIP même si le nombre de sauts IP est plus avantageux que le coût en bande passante. C’est pour cela que la ***distance administrative peut être modifiée*** afin de gérer efficacement notre réseau.

* La route par défaut quand elle est toujours utilisé afin de pouvoir continuer à ***rejoindre des réseaux non présents dans la table de routage***


## II) RIP (petit réseau)

### Versions :

* V1 envoie ses messages en BROADCAST et ne gère aucun sous-réseau

* V2 envoie ses messages en MULTICAST et gère les sous réseau (VLSM)

### Commandes RIP :

-   Activer RIP sur le routeur
```bash
R1(config)#router rip
```

-   Annoncer un réseau voisin
``` bash
R1(config-router)#network @ip_reseau_voisin 
```

-   Changer version de RIP
```bash
R1(config-router)#version 2 
```

-   Désactiver résumé automatique des routes
```bash
R1(config-router)#no auto-summary
```

-   Partager route par défaut
```bash
R1(config-router)#default-information originate
```

-   Empêcher interface de partager mise à jour de la table
```bash
R1(config-router)#passive-interface @interface
```

![[Pasted image 20220831225753.png]]

## II) OSPF (gros réseaux)

### Fonctionnement :

* OSPF fonctionne avec un système de zone afin d’alléger les performances. Un routeur ne connaît que les informations de sa propre zone OSPF, il ne pourra communiquer qu’avec ceux présent dans la même zone. ***L’air BackBone (0)*** joue le rôle de diffuser les informations entre les différentes zones OSPF. 

* L’un des atouts de OSPF est qu’il permet d’ajouter des routes/réseaux apprises via d’autres protocoles comme RIP par exemple ce qui le rend flexible et surtout extensible très facilement. A noter que les adresses MULTICAST ***224.0.0.5*** et ***224.0.0.6*** sont réservées à pour l’envoyer des mises à jour de routage.

* A propos de son fonctionnement, il utilise le paquet “Hello” qui est un message permettant de découvrir les voisins, tenir à jour la table de routage. Ce paquet est envoyé toutes les 10s et si un voisin ne renvoie aucune réponses il est considéré comme DOWN pendant 40s

* Il existe un processus d’élection de deux routeurs dans chaque zone jouant le rôle de délégué et sous délégué qui va la table de routage qui sera transmise à tous les routeurs de la zone. ***Le délégué (DR)*** est le routeur avec la priorité la plus élevée (ip ospf priority), sinon l’ID du routeur le plus élevé (router ID), sinon la loopback avec l’IP la plus haute, sinon l’interface réseau la plus haute.

### Mises à jour:
* Concernant les mises à jour, elles sont transmises uniquement en MULTICAST comme ceci

	-   Les mises à jour envoyées par le DR utilisent l’adresse IP 224.0.0.5
    
	-   Les mises à jour envoyées au DR et au BDR utilisent l’adresse IP 224.0.0.6

### Commandes OSPF :

-   Activer OSPF sur le routeur
```bash
R1(config)#router ospf @ID
```

-   Annoncer un réseau voisin
```bash
R1(config-router)#network @ip_reseau_voisin @masque_inverse area @area`
```

-   Changer priorité de l’interface routeur
```bash
R1(config-router)#ip ospf priority <0-255>
```

-   Changer ID du router
```bash
R1(config-router)#router-id @IP
```

-   Partager route par défaut
```bash
R1(config-router)#default-information originate
```

-   Empêcher interface de partager mise à jour de la table
```bash
R1(config-router)#passive-interface @interface
```
