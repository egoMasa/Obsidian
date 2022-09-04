# Guide IPV4/IPV6 

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

![[Pasted image 20220901130547.png]]
## III) IPV6 

### Type d'adresses 

![[Pasted image 20220901130926.png]]

### Adresses réservées

❗Certaines IP sont automatiquement réservé sur un réseau:

-   **Adresse ::/128** : il s’agit d’une adresse non spécifiée qui n’est jamais assignée à un serveur, mais peut être utilisée comme adresse source en acquisition d’adresse IPv6.
-   **Adresse ::1/128** : c’est l’adresse de loopback équivalent à l’adresse 127.0.0.1 du protocole IPv4.
-   **Adresses 64:ff9b::/96** : il s’agit d’adresses réservées pour les traducteurs de protocoles définit dans la RFC6052.
-   **Adresses ::ffff:0:0/96** : il s’agit d’une représentation d’adresses IPv4 dans une structure particulière d’IPv6. Ces adresses sont utilisées par des logiciels, mais elles ne doivent pas être présentes sur le réseau.
-   **Adresses ::ffff:0:0:0/96** : ce sont des adresses IPv4 traduites pour un usage particulier, décrit par la RFC2765.

### EUI-64 ( configuration automatique )

![[Pasted image 20220901131615.png]]

