# Guide PPP

# 1) Généralités
* ***PPP*** = Point To Point Protocol
* Protocole utilisé lors d'une communication entre 2 équipements via une ***liaison série***. 
* C'est le standard du "Remote Access"
* Il supporte l'encryption et l'authentification
* ETTD = DTE
* ETCD = DCE	
* `show interface serial`
		* `down,down` = Pas branché
		* `up,down` = Pas d'accord
		* `admin down,down` = Shutdown
# 2) LCP et NDP
* ***Couche 2/3*** : 
	* ***LCP (Link Control Protocol)***
		* Etablir/Configurer/Tester connexion de liaison de données
		* Négocier paramètres d'établissement de la liaison
		* Maintient et débogue une liaison
		* Met fin à la liaison
		* Options de contrôle sur liaison de données de réseau étendu

	* ***NCP (Network Control Protocol)***
		* Etablir/Configurer/ protocoles couche réseau
		* Négocie les paramètres protocoles couches 3
		* Peut négocier plusieurs protocoles couches 3
		* Active et désactive les protocoles de couche réseau
		* Permet le fonctionnement de plusieurs protocoles de couche 3 sur la même liaison physique
		* Transporte des paquets à partir de plusieurs protocoles réseau
		* Encapsule et négocie des options pour les protocoles IP et IPX
		* Quand process NCP terminé, la liaison passe à l'état ouvert et LCP prend le relais

# 3) PAP et CHAP

* ***Mécanisme d'authentifications*** : 
	* ***PAP (Password Authentification Protocol)*** 
		* Utilise un échange en 2 étapes pour établir connexion
		* Mot de passe envoyé en clair
		* Vulnérable aux attaques de lecture répétée
	* ***CHAP (Challenge Handshake Protocol)***
		* Utilise un échange en 3 étapes pour établir connexion
		* Vérification périodique
		* Utilise une fonction de hachage unidirectionnelle
		* Le mot de passe configuré avec nom utilisateur router doit être le même sur les deux routeurs

# 4) Fonctionnement PPP

* ***Services optionnels négociés par PPP*** :
	* Authentification : PAP et CHAP
	* Compression : Stacker, Predictor, MPP

* ***Etapes d'établissement connexion PPP***
	* 1) Envoi de trames d'établissement de liaison pour négocier les options (taille MTU, compression, authentification)
	* 2) Envoi de trames de configuration reçu
	* 3) Test de la qualité de la liaison (optionnel)
	* 4) Négociation des options de protocole de couche 3
	* 5) NCP atteint l'état ouvert

* ***HDLC (Trame)***
	* HDLC est utilisé par le protocole PPP comme élément de base pour l'encapsulation de datagrammes sur des liaisons point à point
	* Structure de la trame PPP
	```
	Flag | Address | Control+IDprotocol | Paquets | FCS | Flag
	
	* ID Protocol : Identifie le protocole de couche réseau encapsulé dans le champ de données de la trame
	```

# Commande IOS
* Configurer nom d'utilisateur et le mot de passe
```
username Router2 password cisco
username Router1 password cisco
```
* Passer en mode configuration interface
```
interface serial 0/1/0
```
* Spécifier le type d'encapsulation
```
encapsulation ppp
encapsulation hdlc
```
* Configuration de l'authentification
```
ppp authentification pap
ppp authentification chap
```