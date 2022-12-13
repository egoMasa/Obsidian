# Guide DHCP/DNS

## I) DHCP

### Principe 
* Le DHCP distribue des adresses IP à chaque nouvel appareil sur le réseau qui en demande une. Il peut mettre en place un système de ***réservation d’adresses IP*** pour les appareils habitués si on le souhaite. Il utilise deux ports : 

### Ports utilisés 
* **67** (serveur) 
* ***68*** (client).  

### Méthodes d'attribution 
* Il y a deux modes d’attribution d’adresses : 

	-   ***Statique*** = Une machine à toujours la même IP fixe   
	-   ***Dynamique*** = Une machine obtient une IP disponible non fixe   

### Phases de communication
* Adresse APIPA = Ensemble d’adresses donnée si le client n’obtient pas d’IP du DHCP 
``ex: 169.254.0.0→169.254.255.255``

* DORA (Discover, Offer, Request, Acknowledgement)

	1)Discover (broadcast): Paquet broadcast où le client interroge les DHCP, à ce moment son IP=0.0.0.0, port client 68, port 67 serveur. Le serveur répond à l’adresse MAC de la requête

	2)Offer (Unicast): Le client reçoit une offre avec les paramètres IP

	3)Request (broadcast): Le client informe l’offre à tous le serveur

	4)Acknowledgement (Unicast): Le serveur reçoit l’offre finale et envoie l’accusé fin

### Commandes CISCO
* Création d'un pool DHCP
```cisco
(config)#ip dhcp pool @name
(dhcp-config)#network @ip_reseau @masque
(dhcp-config)#default-router @ip
```
* Durée du bail DHCP
```cisco
(dhcp-config)# lease @jour @heure @seconde
```
* Options gateway et serveur DNS
```cisco
(config)#ip dhcp pool @name
(dhcp-config)#default-router @ip
(dhcp-config)#dns-server @serveur1 @serveur2 ...
```
* Exclusion d'adresses
```cisco
(config)#ip dhcp excluded-address @ip_debut @ip_fin
```
* Detruire pool DHCP
```cisco
(config)#no ip dhcp pool @name
```
* Exemple
```bash
Gateway(config)#ip dhcp pool GW
Gateway(dhcp-config)#network 192.168.1.0 255.255.255.0
Gateway(dhcp-config)#default-router 192.168.1.254
Gateway(dhcp-config)#dns-server 8.8.8.8
Gateway(dhcp-config)#lease 0 8
Gateway(dhcp-config)#exit
Gateway(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10
```

## II) DNS

* Le DNS permet de traduire lors d’une recherche sur un navigateur un nom (ex : instagram) en une IP que l’ordinateur utilise réellement.  Il utilise un système de page jaune en quelque sorte qui lui permet de ***relier un nom à une IP***. Ce protocole fonctionne sous ***UDP*** avec le ***port 53*** pour les requêtes standards
### Commandes DNS
* Activer resolution DNS
```cisco
R1(config)#ip domain-lookup
```
* Désactiver resolution DNS
```cisco
R1(config)#no ip domain-lookup
```
* Configure un resolveur de noms
```cisco
R1(config)#ip name-server @ip
```
* Activer service DNS
```cisco
R1(config)#ip dns server
```