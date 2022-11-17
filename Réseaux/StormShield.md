# Guide StormShield CSNA

# 1) Translation d'adresses NAT

# 2) Filtrage 
## Généralités
* On autorise ou bloque certains flux précis 
* On applique certains critères de passage/blockage
	* IP SOURCE/DESTINATION
	* REPUTATION/GEOLOCALISATION DE L'IP
	* INTERFACE ENTREE/SORTIE
	* ADRESSE RESEAU SOURCE/DESTINATION
	* FQDN (NOM DNS) SOURCE/DESTINATION
	* VALEUR CHAMP DSCP
	* SERVICE TCP/UDP/SCTP avec PORT DE DESTINATION
	* PROTOCOLE IP
	* UTILISATEURS PRECIS
* Certaines inspections de sécurité peuvent êtres activées (analyse antivirale, antispam, filtrage URL)
* Chaque paquet passe par la première règle jusqu'à la dernière pour savoir si il passe
* On créer les règles des plus précises au plus généralistes.
## Stateful
* Technologie SPI (Stateful Packet Inspection) permet de garder en mémoire l'état des connexions TPC/UDP/ICMP pour détecter et suivre les potentielles attaques ou comportement louches.
* Si on autorise un communication de A vers B, implicitement la communication de B vers A est autorisé. Donc il nous faut une règle seulement pour autoriser la requête et la réponse
## Ordonnance du filtrage et de translation
* Priorité des règles pour un paquet initiale
	* 0) ***Filtrage implicite*** : Préconfigurés, ajout dynamique
	* 1) ***Filtrage global*** : Depuis menu SMC (StormShield Management Server)
	* 2) ***Filtrage local*** : Depuis l'interface d'administration 
	* 3) ***NAT implicite*** : Dynamiquement par le Firewall
	* 4) ***NAT global*** : Depuis menu SMC (StormShield Management Server)
	* 5) ***NAT local*** : Depuis l'interface d'administration 
## Menu
* Accéder aux règles implicites
```
CONFIGURATION⇒POLITIQUE DE SECURITE⇒REGLES IMPLICITES
```
* Accéder aux règles globales
```
PROFIL⇒PREFERENCES⇒[x]Display global policies
CONFIGURATION ⇒ POLITIQUE DE SÉCURITÉ ⇒ Filtrage et NAT
```
* Créer règles de filtrage
	* ***Règle simple*** : Filtrage standard
	* ***Règle d'authentification*** : Rediriger connexions d'utilisateurs non authentifiés vers portail captif 
	* ***Règle d'inspection SSL*** : Ajout de règles activation proxy SSL
	* ***Règle de proxy HTTP explicite*** : Ajout de règles activation proxy HTTP explicite
* Code couleur des jauges 
	* ***Blanc*** : Règle jamais appliqué
	* ***Bleue*** : 0-2%  du hit max
	* ***Vert*** : 2-20% du hit max
	* ***Orange*** : Supérieur à 20% du hit maximal
* Descriptions des champs de filtrage
	* ***Etat*** : Activer/Désactiver 
	* ***Action*** : Passer/Bloquer/Tracer/Renvoyer
		* ***Niveau de trace*** : Tracer flux traités par la règle
			* ***Standard*** : Journalisation des connexions  établies uniquement sur la couche transport
			* ***Avancé*** : Journalisation de chaque actions (tentative)
			* ***Alarme mineure/majeure*** : Connexion tracée dans le journal Alarmes avec une alarme mineure/majeure
	* ***Source*** : Source du trafic (point A)
		* ***Utilisateur*** : Renseigne utilisateur/groupe à l'origine du trafic
		* ***Machines sources*** : Indique l'IP, FQDN ou adresse réseau du trafic 
		* ***Interface d'entrée*** : Indique l'interface d'entrée du trafic
		* ***Géolocalisation*** : Renseigne contient/pays à l'origine du trafic
		* ***Réputation IP Publique***  : Groupe BAD
			* Anonymizer
			* Botnet
			* Malware
			* Phishing
			* Scanner
			* Spam 
			* Tor
	* ***Destination*** : Destination du trafic (point B)
	* ***Port*** : Port destination du trafic

# 3) Annuaire authentification
## Introduction
* L'objectif est d'accorder à utilisateurs précis des droits d'accès spécifiques aux réseaux et aux services
* Etapes de configuration sur un Firewall
	* 1) ***Annuaire*** : Stockage des informations de manière hiérarchisé
	* 2) ***Utilisateurs et groupes*** : Stockés dans un annuaire avec des attributs utilisés pour l'authentification
	* 3) ***Méthodes d'authentification*** : Façon de vérifier l'identité de l'utilisateur
	* 4) ***Politique d'authentification*** : Accorder aux utilisateurs des droits d'accès aux réseaux et services
	* 5) ***Portail captif*** : Authentifier/Créer les utilisateur, demande de certificat.
	* 6) ***Politique de sécurité (filtrage)*** : Renvoie et gestion des utilisateurs inconnus vers solution retenue
## Liaison à un annuaire
* 2 catégories d'annuaires :
	* ***LDAP/AD externes*** : Stocké sur serveur externe avec 3 types de d'annuaires supportés
		* ***Microsoft Active Directory (AD)***
		* ***LDAP Standard***
		* ***LDAP de type PosixAccount***
		* ***Champs de configuration*** 
			* ***Nom de domaine*** : DNS du domaine
			* ***Serveur*** : Objet machine ayant l'IP du serveur qui herberge l'annuaire
			* ***Port*** : Port d'écoute du serveur LDAP (389 en clair, 636 en SSL)
			* ***Domaine racine (Base DN)*** : stormshield.eu
			* ***Identifiant/MP*** : Compte admin pour connecter firewall au serveur LDAP externe
			* ***Mot de passe haché*** : L'algorithme de hachage 
		* ***Paramètres annuaire externe***
			* ***Configuration***
				* ***Annuaire distant*** : Paramètres de connexion à l'annuaire (IP, port,...)
				* ***Connexion SSL*** : Activer connexion SSL avec l'annuaire avec autorité de certification spécifiée
				* ***Configuration avancée*** : Définir annuaire de secours et d'autoriser les groupes imbriqués
			* ***Structure***
				* ***Accès en lecture*** : Définit filtres sélection des utilisateurs/groupes dans l'annuaire et indique si l'annuaire est accessible en lecture seule ou lecture/écriture
				* ***Correspondance d'attributs*** : Indique correspondance entre attributs du firewall et ceux du serveur externe
			 * ***Configuration avancée***
				 * ***Caractères protégés*** : Définit les caractères qui doivent être protégés dans les requêtes LDAP
				 * ***Hachage de MP*** : Selection de l'algorithme de hachage
				 * ***Utilisateurs et groupes*** : Renseigne les branches où sont enregistrés les utilisateurs et groupes crées à partir du firewall
				 * ***Autorité de certification*** : Définit l'emplacement de l'autorité de certification de l'annuaire externe

	* ***LDAP interne*** : Stocké sur le firewall et héberge les utilisateurs
		* ***Champs de configuration*** 
			* ***Organisation*** : Nom de l'organisation
			* ***Domaine*** : TLD du domaine (ex: .eu)
			* ***Mot de passe*** : MP de connexion à l'annuaire depuis navigateur LDAP
			* ***Mot de passe haché*** : L'algorithme de hachage 
			* ***Configuration*** : 
				* ***Utilisation de l'annuaire utilisateur*** : Démarrer service LDAP
				* ***Mot de passe*** : Mot de passe permettant de se connecter à l’annuaire
				* ***Activer l'accès non chiffré (PLAIN)***
				* ***Activer l'accès SSL*** : Renseigner certificat SSL présenté par le serveur
				* 
* Ajout et configuration d'un annuaire
```
CONFIGURATION ⇒ UTILISATEURS ⇒ Configuration de l'annuaire.
```
## Gestion des utilisateurs
* Visualiser utilisateurs/groupes
```
CONFIGURATION⇒ UTILISATEURS ⇒ Utilisateurs
```
* Un utilisateur possède trois attributs 
	* Compte
	* Certificat
	* Groupes
* Accorder des droits utilisateurs
```
CONFIGURATION⇒ UTILISATEURS ⇒ Droits d'accès ⇒ onglet ACCÈS DÉTAILLÉ
```
* Ajouter/Supprimer utilisateurs
```
CONFIGURATION ⇒ UTILISATEURS ⇒ Utilisateurs
```
## Portail captif
* Accéder au portail captif
```
https://@ip_fw/auth
```
* Permet plusieurs actions
	* Enregistrer nouveaux utilisateurs
	* Authentifier utilisateur et lui permettre l'accès au réseau
	* Demande de certificat
* Configurer portail captif
```
CONFIGURATION ⇒ UTILISATEURS ⇒ Authentification ⇒ onglet PORTAIL CAPTIF
```
* Déconnecter utilisateur connecté
```
SUPERVISION ⇒ UTILISATEURS
```
* Enrolement : Permet aux utilisateurs de s'enregistrer en remplissant formulaire d'inscription qui envoie une demande à l'administrateur dans :
```
CONFIGURATION ⇒ UTILISATEURS ⇒ Enrôlement
```
## Méthodes d'authentification
* Accèder à la politique d'authentification
```
CONFIGURATION ⇒ UTILISATEURS ⇒ Authentification ⇒ onglet POLITIQUE D’AUTHENTIFICATION
```
* Créer une règle d'authentification LDAP
## Politique d'authentification
* Il faut indiquer une politique afin d'indiquer les méthodes en fonction des utilisateurs
* 
## Règle de filtrage pour l'authentification
* Pour rediriger les utilisateurs inconnus vers le portail captif. 
* Il faut impérativement créer des règles pour laisser passer ce qui sont authentifiés.
* 
## Définir nouveaux administrateur



# 4) IPSEC
Voir VPN
