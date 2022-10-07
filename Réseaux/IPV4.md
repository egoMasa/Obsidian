# Guide IPV4

## I) Principe et explication
* Sur un réseau, chaque appareil ou équipement informatique possède une adresse IP qui lui est propre. Cette adresse IP va jouer le rôle de ***numéro d’identification*** qui va permettre ***l’acheminement des paquets***. 

* On appelle ça une adresse IPV4 car elle correspond au système IP (***Internet Protocol***) qui est le protocole de tout réseau informatique utilisant internet. 

* Il existe plusieurs type d’adresses IP :

	-   ***IPV4*** (IP version 4) = ***32 bits*** soit 4 octets, format decimal ( ex : ***172.16.254.1***)

	-   ***IPV6*** (IP version 6) = ***128 bits*** soit 16 octets, format hexadécimal ( ex : ***2001:db8:3c4d:15::1a2f:1a2b***) avec masque de /64


## II) IPV4 

### Définition

![[Pasted image 20220901130340.png]]

### Conversion 

![[Pasted image 20220901131915.png]]


❗Certaines IP sont automatiquement réservé sur un réseau

-   IP du réseau (***192.168.1.0***)

-   IP broadcast (***192.168.1.255***)

### Classe IPV4

* Classe A = 0-127 où 127 utiliser pour loopback
* Classe B = 128-191
* Classe C = 192-223
* Classe D = 224-239 
* Classe E = 240-255 (internet expérimental)

![[Pasted image 20220906221541.png]]