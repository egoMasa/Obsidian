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
