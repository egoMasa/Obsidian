# Guide VPN (Virtual Private Network)

## I) Principe et explication

* Un VPN est un réseau de communication privé qui utilise un réseau public (tel que l'internet) pour établir une connexion à distance
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


## II) Familles de VPN
* VPN SSL = Client nomade
* VPN IPsec = Site à site ou client à nomade
	* Assure L'authentification via clé pré-partagée PSK ou par certificats PKI
	* Assure l'intégrité : Vérifie données non modifiées en utilisant les algorithmes de hachage.
	* Assure la confidentialité : Que les données ne peuvent être lues que par une personne tierce capturant le trafic
	* Assure l'anti-rejeu : Permet d'ignorer des anciens paquets dont le numéro de séquence est antérieur à un certain seul déjà reçu, s'ils sont transmis à nouveau
### 1)Site-to-site
* Communication entre 2 entreprises via Internet
* Les données sont cryptés en entrée et décrypter en sortie

### 2)Host-to-site
* Communication entre client et une entreprise via Internet
* Les données sont cryptés après que le client soit connecter au VPN 
* WebVPN, ClientVPN

### 3)Host-to-Host
* Communication entre 2 clients via Internet
* Pas besoins d'équipement réseau supplémentaire mais juste un logiciel VPN
* Tunnel SSH, HTTPS, etc

## III) Protocole VPN

### PPTP (Point-to-Point Tunneling Protocol)
* Le plus ancien protocole 
* Peu sécurisé 
* Obsolète

### L2TP/IPsec (Layer2 Tunneling Protocol)
* Fonctionne par dessus IPsec (système de cléf)
* Système de multi-authentification +  sécurisation et du chiffrement des données grace à IPsec
* Possibilité d'un tunneling en mutli-threading et donc l'exécution de plusieurs tâches sécurisées en simultané

### IKEv2 (Internet Key Exchange version 2)
* Basé sur IPsec (système de cléfs)
* Identifie les deux bouts du tunnel IPsec
* On l'utilise souvent sur mobile grâce à sa gestion du passage d'un réseau wifi peu sécurisée vers le réseau de données mobiles
### OpenVPN
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
