# Guide VPN (Virtual Private Network)

## I) Principe et explication

* Un VPN est un réseau de communication privé qui utilise un réseau public (tel que l'internet) pour établir une connexion à distance
* Un VPN est utilisé pour créer une connexion sécurisée entre deux ordinateurs ou réseaux distants, afin de permettre aux utilisateurs de partager des données ou d'accéder à des ressources en toute sécurité
* Il crypte les données lors de l'envoi et les décrypte lors de la réception.
* Fournit une liaison spécialisée entre deux points sur l'internet
* L'équipement "VPN Concentrateur" créer la connexion VPN et gère l'acheminement des messages entre les ordinateurs et les dispositifs VPN. Il authentifie les utilisateurs, crypte les données, et attribue des adresses IP de tunnel aux utilisateurs. Néanmoins il ne sont pas obligatoire on l'utilise que pour les grosses entreprises. On peut utiliser des logiciels VPN sur d'autres équipements qui assure d'autres taches comme un firewall ou un routeur
* La négociation du tunnel s'effectue via le protocole ISAKMP (Internet Security Association Key Management Protocol) appelé aussi IKE (Internet Key Exchange) transmit via le protocole UDP sur le port 500
* Les extrémités de trafic peuvent communiquer via le protocole ESP (Encapsuling Security Payload) qui assure la confidentialité et l'intégration des données. ESP est encapsulé directement dans un paquet IP
* Deux modes de fonctionnement conditionnent la décision d'encapsuler les paquets IP dans ESP :
	* Correspondance de politique (standard) :
		* Concordance IP usagers avec politique VPN IPSec
		* Critères IP source + IP destination
		* Politique IPSec évaluée en amont des directives générales de routage IP. 
		* Repose exclusivement sur correspondance de politique
	* Virtual Tunneling Interface (si routage VTI activé)
		* Routage via VTI distante dont l'IP appartient au même réseau VTI locale
		* Interfaces VTI permettent de définir des routes passant par tunnel IPSec
		* Agissent comme passerelles réciproques l'un l'autre (point d'entrée et de sortie du tunnel)
### Phase d'authentification 
* Durant l'authentification, chaque extrémité vérifie l'identité de l'autre. 
	* 1) L'adresse IP du réseau externe (Firewall_out) si adresse fixe
	* 2) Un FQDN si une extrémité ne possède pas d'adresse IP fixe (dynamique)
* Selon la méthode d'authentification utilisée, l'identité peut être associé à :
	* Une clé PSK : Chaque extrémité montrera une preuve quelle détient la PSK commune
	* Une PKI : Chaque extrémité présentera un certificat numérique X509 signé par une autorité de certification de confiance.
### Phase d'établissement d'un tunnel VPN IPSec
* Phase 1 : 
	* Les deux extrémités négocient un profil de chiffrement phase 1 avec dedans les algorithmes de chiffrement/authentification. 
	* Les deux extrémités s'authentifie via PSK ou PKI. 
	* Un dialogue d'application chiffré nommé ISAKMP-SA (IKEv1 ou PARENT-SA) est établi qui permet la négociation de la phase 2 qui sera chiffré via la clef ISAKMP-SA
* Phase 2 : 
	* Les deux extrémités négocient un profil de chiffrement phase 2 et pourront communiquer via le tunnel VPN IPSec
	* Deux canaux sont ouverts pour la transmission de données (dans chaque direction)
	* Chaque canal utilise une clef de chiffrement qui lui est propre (ESP-SA1 ou CHILD-SA1)
	* Les deux extrémités possèdent les deux clefs symétriques, une qui chiffre, l'autre qui déchiffre.


# II) Différents types de VPN (SSL/IPSEC/MPLS)

## 1) VPN SSL (Secure Sockets Layer)
* Le protocole SSL utilise le chiffrement ***SSL/TLS*** pour sécuriser les données qui transitent sur le réseau.
* Le protocole SSL s'exécute au niveau de la ***couche de transport*** (couche 4 du modèle OSI) et ne nécessite pas de logiciel client spécial pour être utilisé.
* Le protocole SSL est généralement plus compatible avec les différents systèmes d'exploitation et navigateurs web, ce qui le rend plus facile à utiliser.
* Fonctionnement VPN SSL
	* 1.  Lorsqu'un utilisateur veut se connecter à un réseau VPN SSL, il envoie une requête au serveur VPN.
	* 2.  Le serveur VPN vérifie l'identité de l'utilisateur et envoie une réponse de connexion sécurisée au client.
	* 3.  Le client utilise le protocole SSL pour établir une connexion sécurisée avec le serveur VPN.
	* 4.  Une fois la connexion établie, toutes les communications entre le client et le serveur VPN sont chiffrées et sécurisées grâce au protocole SSL.
* Permet aux utilisateurs de se connecter à un réseau d'entreprise de manière sécurisée depuis n'importe où

### Précision sur SSL
* Protocole de sécurité qui permet de sécuriser les communications sur Internet en chiffrant les données qui transitent entre un client et un serveur.
* Fonctionnement du protocole SSL
	* 1.  Lorsqu'un utilisateur veut accéder à un site Web sécurisé via SSL, il envoie une requête au serveur Web.
	* 2.  Le serveur Web envoie au client une copie de son certificat SSL, qui contient des informations de chiffrement et d'authentification.
	* 3.  Le client vérifie l'authenticité du certificat SSL et envoie une réponse au serveur Web.
	* 4.  Le serveur Web envoie une clé de chiffrement au client.
	* 5.  Le client utilise la clé de chiffrement pour chiffrer toutes les communications qu'il envoie au serveur Web.
	* 6.  Le serveur Web déchiffre les communications en utilisant la même clé de chiffrement.

## 2) VPN IPsec (Internet Protocol Security) 
* Le protocole IPsec utilise un ensemble de protocoles de chiffrement pour sécuriser les données, notamment :
	* ***AH*** (Authentication Header) 
	* ***ESP*** (Encapsulating Security Payload)
* Le protocole IPsec s'exécute au niveau de la couche réseau (couche 3 du modèle OSI) et nécessite généralement l'installation d'un logiciel client spécial sur les ordinateurs qui souhaitent se connecter au réseau VPN.
* Le protocole IPsec peut être plus difficile à configurer et à utiliser sur certains systèmes d'exploitation et navigateurs web.
* Le protocole IPsec est généralement considéré comme plus sécurisé que le protocole SSL car il utilise un ensemble de protocoles de chiffrement plus complet.

### Précision sur IPsec
* Protocole de sécurité qui permet de sécuriser les communications sur un réseau privé virtuel (VPN) ou sur Internet
* S'exécute au niveau de la couche réseau (couche 3 du modèle OSI)
* Utilise un ensemble de protocoles de chiffrement pour sécuriser les données qui transitent sur le réseau
	* ***AH*** (Authentication Header) 
	* ***ESP*** (Encapsulating Security Payload)
* Fonctionne en associant une paire de clés de chiffrement à chaque paquet de données qui transitent sur le réseau.
* Les clés de chiffrement sont utilisées pour chiffrer et déchiffrer les données
* Les ordinateurs qui souhaitent se connecter au réseau VPN doivent être configurés avec un logiciel client IPsec spécial qui gère la génération et l'échange des clés de chiffrement, ainsi que la gestion des communications sécurisées sur le réseau VPN.

### Précision sur AH
* Fait partie du protocole IPsec
* S'exécute au niveau de la couche de réseau (couche 3 du modèle OSI)
* Permet de sécuriser les communications sur un réseau privé virtuel (VPN) ou sur Internet.
* Fonctionne en ajoutant une en-tête d'authentification aux paquets de données qui transitent sur le réseau.
	* Informations d'authentification qui permettent de vérifier l'intégrité des données
	* Protéger contre la modification ou la suppression non autorisée des données
* Le protocole AH s'occupe de l'authentification des données tandis que le protocole ESP s'occupe du chiffrement et de la déchiffrement des données.
* Le protocole AH ne s'occupe pas du chiffrement des données !
* Utilisation du protocole AH
	* 1.  Lorsqu'un paquet de données arrive sur le réseau VPN ou sur Internet, il est acheminé vers le routeur ou le pare-feu qui gère la connexion VPN.
	* 2.  Le routeur ou le pare-feu analyse le paquet de données et utilise le protocole AH pour ajouter une en-tête d'authentification au paquet de données. Cette en-tête contient des informations d'authentification qui permettent de vérifier l'intégrité des données et de protéger contre la modification ou la suppression non autorisée des données.
	* 3.  Le paquet de données modifié est envoyé au prochain routeur ou pare-feu sur le chemin de la destination finale.
	* 4.  Lorsque le paquet de données arrive à sa destination finale, le routeur ou le pare-feu de destination vérifie les informations d'authentification de l'en-tête AH pour s'assurer que les données n'ont pas été modifiées ou supprimées en cours de route. Si les informations d'authentification sont correctes, le paquet de données est envoyé au destinataire final.

### Précision sur ESP
* Fait partie du protocole IPsec
* S'exécute au niveau de la couche de réseau (couche 3 du modèle OSI)
* Permet de sécuriser les communications sur un réseau privé virtuel (VPN) ou sur Internet.
* Fonctionne en encapsulant les paquets de données qui transitent sur le réseau dans un paquet ESP sécurisé.
	* Contient des informations de chiffrement
	* Permettent de chiffrer et de déchiffrer les données
	* Informations d'authentification pour vérifier l'intégrité des données
* Souvent utilisé conjointement avec le protocole AH
* Utilisation du protocole ESP
	* 1.  Lorsqu'un paquet de données arrive sur le réseau VPN ou sur Internet, il est acheminé vers le routeur ou le pare-feu qui gère la connexion VPN.
	* 2.  Le routeur ou le pare-feu analyse le paquet de données et utilise le protocole ESP pour encapsuler le paquet de données dans un paquet ESP sécurisé. Ce paquet ESP contient des informations de chiffrement qui permettent de chiffrer et de déchiffrer les données, ainsi que des informations d'authentification qui permettent de vérifier l'intégrité des données.
	* 3.  Le paquet ESP est envoyé au prochain routeur ou pare-feu sur le chemin de la destination finale.
	* 4.  Lorsque le paquet ESP arrive à sa destination finale, il est déchiffré et analysé par le routeur ou le pare-feu de destination. Si les informations d'authentification sont correctes, le paquet de données est envoyé au destinataire final.

## 3) VPN MPLS
* Type de réseau privé virtuel qui utilise le protocole MPLS pour acheminer les données entre les différents sites d'une organisation.
* Voici comment un VPN MPLS fonctionne en détail :
	* 1.  Les données sont acheminées entre les sites d'une organisation via un réseau MPLS privé qui est géré par un fournisseur de services.
	* 2.  Les données sont acheminées sur le réseau MPLS en utilisant des étiquettes (ou labels) qui sont ajoutées aux paquets de données. Ces étiquettes indiquent au réseau MPLS où acheminer les données.
	* 3.  Les données sont acheminées sur le réseau MPLS de manière efficace en utilisant des algorithmes de routage qui permettent de sélectionner le meilleur chemin pour acheminer les données.
	* 4.  Les données sont délivrées à leur destination finale en enlevant les étiquettes et en utilisant l'adresse IP de destination.

# III) Différents protocoles VPN

* Il existe de nombreux protocoles VPN différents qui peuvent être utilisés pour établir une connexion VPN.

## PPTP (Point-to-Point Tunneling Protocol)
* Le plus ancien protocole 
* Peu sécurisé 
* Obsolète

## L2TP(Layer2 Tunneling Protocol)
* Fonctionne par dessus IPsec (système de cléf)
* Plus sécurisé que PPTP
* Plus complexe à configurer

## SSTP (Secure Socket Tunneling Protocol)
* Utilise SSL pour chiffrer les données et établir une connexion sécurisée. Il est souvent utilisé par les entreprises pour se connecter à des réseaux d'entreprise de manière sécurisée, mais il nécessite généralement l'installation de logiciel spécial sur les ordinateurs client.

## IKEv2 (Internet Key Exchange version 2)
* Permet d'établir une connexion sécurisée entre deux points sur un réseau.
* Très adapté aux connexions mobiles
* Utilise une clé de session commune pour chiffrer les communications.
* Voici comment le protocole IKEv2 fonctionne en détail :
	* 1.  Lorsqu'un utilisateur veut établir une connexion VPN à l'aide d'IKEv2, il envoie une requête au serveur VPN.
	* 2.  Le serveur VPN envoie au client une invitation à établir une connexion sécurisée en utilisant le protocole IKEv2.
	* 3.  Le client envoie une réponse au serveur VPN en utilisant le protocole IKEv2 pour établir une session de négociation de clé.
	* 4.  Le serveur VPN et le client échangent des informations de clé et de chiffrement, et établissent une clé de session commune pour chiffrer les communications.
	* 5.  Une fois la clé de session établie, le serveur VPN et le client peuvent échanger des données de manière sécurisée en utilisant le protocole IKEv2.
## OpenVPN
* Le plus populaire
* Rapide et fiable
* Utilisent majoritairement AES 256 bits pour la méthode chiffrement

# IV) VRF (Virtual Routing and Forwarding)

* Segmenter un routeur en plusieurs couche (toujours sur le même)
* Meme principe qu'un VLAN 

## Commandes 
* Créer les VRF
```
ip vrf @nom_vrf
	description @desc
```
* Associer VRF à une interface
```
int G0/0
	ip vrf forwading @nom_vrf
	ip address ...
```
* Configurer protocole de routage associé à une VFR
```
router ospf 1 vrf @nom_vrf
```
* Configurer route statique
```
ip route vrf @nom_vrf @reseau_desti @masque @saut
```
* Verification
```
sh ip route vrf @nom_vrf
ping vrf @nom_vrf @ip
```
