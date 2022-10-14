# Guide Firewall 

# I) Principe et explication
* Il s'agit d'un élément logiciel ou physique qui réglemente les droits de passage des connexions entre des zones séparer physiquement.
* Il met en oeuvre une politique de sécurité définie par une matrice de flux
* Schéma de base d'un firewall :
![[Pasted image 20220924163405.png]]


# II) Types de Firewall
### 1) Firewall stateless (sans état)
* ***Philosophie*** : 
Traite chaque paquet comme une unité isolée et sans interet à l'état de connexion
* ***Décision de filtrage*** :
Filtre sur la base des informations d'en-tête d'un paquet, comme l'adresse IP source, l'adresse IP de destination, le numéro de port, etc.
* ***Performances*** :
Assez faible en consommation et qualité mais rapide
* ***Statut de la connexion***:
Inconnu
* A savoir :
Il utilise un système règle basiques type ACL qui bloque ou autorise des IPs ou Ports. Il sépare en deux partie une connexion (request, response) et ne les relie pas entre elle. Il faut donc 2 règles par entrée et par sortie, donc 4 au total
![[Pasted image 20220924171249.png]]
### 2) Firewall stateful (avec état,dynamique)
* ***Philosophie*** : 
Maintien le contexte des sessions actives. Utilise le concept d'une table d'état où il stocke l'état des connexions légitimes. Inspecte tout ce qui se trouve dans les paquets de données, les caractéristiques des données et leurs canaux de communication. 
* ***Décision de filtrage*** :
Filtrent les paquets en les faisant correspondre à des états valides dans la table d'état.
* ***Performances*** :
Elevée en consommation et qualité mais moins rapide
* ***Statut de la connexion***:
Connu
* ***A savoir*** :
Si un requete envoyé sur un port précis (ex : 80 du serveur) et que la reponse vient d'un autre port du serveur la requête sera bloqué car les fw stateful garde les adresses/ports en memoire et tout ce qui est inconnue ou louche comme reaction est banni. Il est capable de relier une connexion de bout en bout (request et reponse) et il suffit d'une seule règle d'entrée ou de sortie.
![[Pasted image 20220924171316.png]]

# III) Passerelle sécurisée (DMZ)
* On utilise une zone DMZ qui joue le role de zone tampon entre deux firewalls. Il permet de limiter les dégats si un firewall ne fonctionne pas comme il le devrait.
* Cette passerelle assure le relai des flux, gestions d'athentification et des autorisations, Détections d'intrusions, Filtrage contenues indésirables, Journalisation des logs. Le but est de protéger de facon encore plus sécurisé une zone bien précise reculé dans le réseau et de laisser une partie avec un seul firewall comme défense (des services non comprometant)

# IV) Politique de filtrage
