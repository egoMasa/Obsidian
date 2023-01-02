# Guide Ethernet (CSMA/CD/CA)

* Technologie support partagé 
* Accès concurrentiel
* Principe du premier arrivé, premier servi
* Gère les collisions

# Méthode d'accès MAC (CSMA/CD)
* Premier arrivé, premier servi
* Si canal libre, la station place son traffic sinon elle attend
* Gère les collisions
* Pas de fonction de fiabilité (ACK), gestion d'erreurs ou controle de flux

# Explication du CSMA (Carrier Sense Multiple Access)
1) Une interface veut communiquer et écoute le support
2) Si porteuse, retardement de sa communication
3) Si pas porteuse, le support est libre et elle attend un court instant avant de placer le trafic
4) Attente d'éventuelles collisions
5) Canal considéré comme acquis et envoie de données
6) En cas de media partagé, toutes les interfaces reçoivent les informations de chaque trafic, elles examinent l'en tête de la trame et garde ce qui est destiné à eux.

# Gestion des collisions (CD)
* Une collision survient quand 2 interfaces sur un support partagé tentent de placer une trame en meme temps car elles ont determiné le support comme libre (sans porteuse)
* Cela arrive dut au delai pour qu'une trame arrive d'un extremité à l'autre support.
* En cas de collision
	* Les stations impliquées envoyent un signal de bourrage afin que les interfaces du réseau l'entendent
	* Elles attendent de pouvoir reprendre leur procédure.

# Ethertype
* Correspond à la charge contenu dans la trame, selon son type elle varie
* ***0x0800*** : Internet Protocol version 4 (***IPv4***)
* ***0x0806*** : Address Resolution Protocol (***ARP***)
* ***0x8100*** : VLAN-tagged frame (***IEEE 802.1Q***)
* ***0x86DD*** : Internet Protocol Version 6 (***IPv6***)
* 0x8863 : PPPoE Discovery Stage
* 0x8864 : PPPoE Session Stage
* 0x8870 : Jumbo Frames
* 0x888E : EAP over LAN (IEEE 802.1X)
* 0x9100 : Q-in-Q

# Adressage MAC 802
* Adresse correspondant à l'***adresse physique de la carte***
* ***48 bits*** en ***hexadécimal***
```
AA:BB:CC:DD:EE:FF
```
* ***24 premiers bits*** = Adresse MAC qui ***identifie le constructeur***, aussi appelé OUI (Organizationaly Unique Identifier)
* 24 derniers bits sont laissés à la discrétion du constructeur
* On trouve des adresses MAC 
	* Unicast (Une interface)
	* Broadcast (toutes les interfaces) : ``FF:FF:FF:FF:FF:FF``
	* Multicast (Groupe)