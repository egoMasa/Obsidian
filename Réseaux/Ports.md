# Guide ports 

## I) Principe 
* Un **port** peut désigner : **port** matériel(physique), une prise permettant de brancher des périphériques sur un ordinateur ; **port** logiciel, un système permettant aux ordinateurs de recevoir ou d'émettre des informations en fonction des protocoles

## II) Ports matériel
* Ports internes (sur carte mère)
	* PCI
	* SATA
	* ISA
* Ports externes
	* USB
	* SATA
	* Serie
* Audio/Vidéo
	* Displayport
	* DVI
	* HDMI
	* Jack
	* VGA
* Réseaux
	* RJ45 (Ethernet)
## III) Ports logiciel
* Correspondant à la couche de transport ***Couche de transport (4)***
* Protocoles ***TCP*** et ***UDP***
* Un numéro de port est codé sur 16 bits, soit ***65 536*** divisé en 3 parties :
	* ***0 à 1 023*** correspondent aux ports "bien-connus" (well-known ports), utilisés pour les services réseaux les plus courants,
	- ***1 024 à 49 151*** correspondent aux ports enregistrés (registered ports)
	- ***49 152 à 65 535*** correspondent aux ports dynamiques, utilisables pour tout type de requêtes TCP ou UDP autres que celle citées précédemment.

### Liste ports well-known

-   ***20/21*** = Echange de fichiers via ***FTP***
-   ***22*** = Connexion ***SSH*** et échange de fichiers sécurisés ***SFTP***
-   ***23*** = Connexion ***telnet***
-   ***25*** = Email ***SMTP***
-   ***53*** = ***DNS***
-   ***67/68*** =  ***DHCP***
-   ***69*** = ***TFTP*** (transfert de fichier)
-   ***80*** = ***HTTP*** (requete web)
-   ***443*** = ***HTTPS*** (securité SSL)
-   ***500*** = ***IPsec*** (VPN)