
# Comprendre la différence entre la commutation et le routage 

# I) Commutation
## Principe
* La commutation est une fonctionnalité qui achemine les données sous forme de trame d'un point A vers une point B au sein d'un même réseau
* Connecte plusieurs périphériques
* Il est effectué par les commutateurs(switches)
	* Garde en mémoire les MAC relié à chacune de ses interfaces
* Il est basé sur l'adresse MAC de destination contenu dans la trame 
## Fonctionnement
* Le commutateur recoit la trame
* Il extrait l'adresse MAC de destination
* Il la compare par rapport à sa table MAC
* Il relie l'adresse MAC à une interface physique
* Il commute la trame sans modifications sur cette interface permettant d'accéder à l'adresse MAC de destination

## Exemple de commutation
* Un hôte envoie une trame vers un autre hôte sur le même réseau 
* Le switch passerelle envoie la trame qu'il vient de recevoir un chacun de ses voisins
* Les prochains switches font de même 
* Une fois réceptionné au bon hôte, sur le chemin retour (réponse) la trame ne se diffuse plus et revient directement ) l'origine

# II) Routage
## Principe
* La commutation est une fonctionnalité qui achemine les données sous forme de trame d'un point A vers une point B situé dans un réseau distant
* Connecte plusieurs réseaux
* Il est effectué par les routeurs
* Il est basé sur l'adresse IP de destination contenu dans le paquet reçu 
* On choisit via des algorithmes de routages quel chemin est le plus court/rapide
* L'adresse MAC est détruit à l'entrée du routeur et est reconstruite à la sortie.
## Fonctionnement
* Le routeur recoit un paquet
* Il extrait l'adresse IP de destination 
* Il compare l'adresse IP de destination à sa table de routage
* Il envoie se paquet vers un routeur voisin précis
* Le routeur final extrait l'adresse IP de destination et compare à sa table
	* L'IP correspond à un réseau relié à une de ses interfaces 
## Exemple de routage 
1) Routage sur ***liaison serie***
	* Lors d'une connexion serie P2P, le routeur encapsule le paquet dans le ***format de trame de liason*** utilisé par ***l'interface de sortie*** (HDLC,PPP)
	* On doit utiliser une adresse de destination de diffusion de couche 2 si aucune adresse MAC sur les interfaces série
	* Aucunes adresse source n'est requise
1) Routage sur ***liaison normal***
	* Les adresses ***MAC de source et destination*** change à chaque saut
	* La source MAC est celui qui viens de ***recevoir le paquet***
	* La destination MAC est l'interface du ***prochains saut***
	* ***l'IP source et de destination ne change jamais***