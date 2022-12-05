# Guide StormShield CSNA

# 0) Introduction
* On placera les ***serveurs publics*** (web, ftp, mail, etc.) dans la ***DMZ***
* Le firewall Stormshield peut servir de serveur DHCP pour les machines du réseau local
* On doit se connecter sur une interface interne
	* HTTPS (443/TCP)
* On peut modifier le port SSH (22/TCP)
* Active Update interroge les srvs de MAJ
* Caractéristiques de la configuration d'usine :
	* Un serveur DHCP est actif sur les interfaces du brige et distribue un pool compris entre : 10.0.0.10-10.0.0.100
	* La configuration réseau est en mode transparent (Bridge)
* Tous les Firewall Stormshield disposent d'un port Ethernet Gigabit
* Avec la RGPD, seul le super administrateur "admin" peut accéder aux logs complets en cliquant sur "Accès Restreint aux logs"
* Toutes les interfaces sont réunies dans un bridge ayant l'adresse 10.0.0.254/8
* La première interface du firewall est nommée « OUT », la deuxième est est "IN"
* Le nom du firewall est son numéro de série

# 1) Traces et supervision

## Catégorie de traces
* On a 4 catégorie de traces 
	* PROXY
		* POP3
		* SMTP
		* FTP
		* SSL
		* HTTP
	* Connexion
		* Réseaux
		* Applicatives
		* Authentification
		* Politique de filtrage
	* VPN
		* IPsec
		* SSL
	* Système
		* Administration
		* Evenements
	* Sécurité et usage
		* Stats
		* Vulnérabilités
		* Alarmes
		* SandBoxing

## Configuration des traces

* On peut configurer l'enregistrement de ces événements
	* Activer/Désactiver l'enregistrement
	* Sélectionner le support de stockage
	* Actualiser
	* Formater
	* Espace réserver par catégorie
	* 
# 0) Objets

## Généralités
* Les objets commençant par ***Network_*** ne sont pas modifiables
* L'équivalent de ***Internet*** est ***Différent de Network_Internals***
* On peut éditer une catégorie d'URL pour lire son contenu 
* Les objets routeurs sont utilisées pour :
	* Le routage par défaut
	* Le routage par politique
* Les objets Routeur permettent de : 
	* Tester la disponibilité des passerelles
	* Configurer un routage avec répartition de charge sur plusieurs passerelles
* Le nom d'un objet peut :
	* Contenir nombres de caractères illimités
	* Commencer par un chiffre
	* Etre un nom de domaine

# 0) Réseau

## Modes de configuration

* 3 Modes de configuration
	* Transparent/Bridge (On ne différencie pas les réseaux)
	* Avancé/Routeur (On différencie chaque réseau)
		* Gère plusieurs réseaux logiques 
		* Segmentation du réseau aux niveaux logique et physique
		* On doit gérer un PAT pour accéder à internet sur l'interface OUT
	* Hybride (On différencie uniquement le OUT ou inversement)
		* Plusieurs interfaces dans un bridge
		* Autres interfaces indépendantes du bridge
		* 
## Type d'interfaces
* 5 types d'interfaces sir le FW
	* ***Physique*** : Dépend du modèle
	* ***Bridge*** (nv 2) : Association de plusieurs interfaces physique ou VLAN
	* ***VLAN*** : Nombre max dépend du modele
	* ***Modem*** : Connexion entre FW et un modem (ADSL)
		* Connexion possibles sous PPPoe et PPTP
	* ***GRETAP*** : Encapsulation qui relie 2 réseaux distants au niveau 2 (bridge)
	* ***LAGG*** : 8 Max
* 2 type de protection
	* ***Interne*** : Protégée et n'accepte que les paquets provenant d'un plan connu (directement co ou via route statique)
	* Externe : Publique et non protégée, peut recevoir des paquets de n'importe qui, on l'utilise pour connecter le firewall à Internet

## Routage système
* Par défaut : Si trafic ne correspond à aucunes routes dans la table de routage
	* NETWORK/ROUTING/IPV4 STATIC ROUTE
* Statique : Renseigne manuellement la passerelle vers un réseau distant
	* NETWORK/ROUTING/IPV4 STATIC ROUTE
		* Réseau/GW/Interface_sortie_vers_gw

## Routage avancée
* Dynamique
	* BIRD : Logiciel qui met en œuvre le routage dynamique
		* RIP
		* BGP
		* OSPF
	* Politique (Policy Based Routing PRB)
		* Spécifie la GW dans règle de filtrage
		* Flux spécifié envoyé vers GW choisie
* Répartition de charge
	* Répartir connexions sortantes vers différentes GW
	* Soit équitable ou pondérée avec pourcentage du trafic global
	* Basé soit sur l'IP source ou les paramètres de connexions
* Passerelles de secours
	* Cas où une passerelle ne fonctionne pas
	* 64 passerelles de secours maximum
* Route de retour
	* Spécifier l'interface de sortie pour atteindre GW distante
	* Force le flux sortant d'un connexion entrante à repasser via l'interface d'entrée de connexion
	* Forcer le flux à reprendre la même route que par là où il est rentré

## Ordonnance des types de routages
* Ordre de priorité
	* 1) Route de retour
	* 2) Routage par politique
	* 3) Routage statique
	* 4) Routage dynamique
	* 5) Routage par défaut
# 1) Translation d'adresses NAT
## Généralités
## Translation dynamique
* Une seule IP publique (pub_fw) pour le réseau privé
* Une machine est précise prend un port précis [20000-59999] de l'unique adresse publique
* Choix du port de façon séquentiel
## Translation statique par port
* Externaliser les serveurs LAN sur l'IP publique du FW et un port précis et relier à un serveur
## Translation statique
* Une seule IP publique pour chaque serveur interne
## Menu NAT
* `SECURITY POLICY/FILTER-NAT`
* ***Translation dynamique 
	* Nouvelle Règle / Règle de partage d'adresse source (masquerading)
* ***Translation statique par port***
	* Règle standard
		* Source : Internet
		* Destination_original : pub_fw port 80
		* Destination_after : srv_web_priv
* Translation statique
	* Nouvelle règle / Règle de NAT statique (bimap)
		* On créer un objet srv_pub et srv_priv

## Ordre d'application des règles

## Avancées

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
	* 1) ***Filtrage implicite*** : Préconfigurés, ajout dynamique
	* 2) ***Filtrage global*** : Depuis menu SMC (StormShield Management Server)
	* 3) ***Filtrage local*** : Depuis l'interface d'administration 
	* 4) ***NAT implicite*** : Dynamiquement par le Firewall
	* 5) ***NAT global*** : Depuis menu SMC (StormShield Management Server)
	* 6) ***NAT local*** : Depuis l'interface d'administration 
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

# 4) IPSEC
Voir VPN
