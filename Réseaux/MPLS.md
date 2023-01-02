# Guide MPLS

# 1) Principe MPLS
* MPLS = ***Multiprotocol Label Switching***
* Réseau privé et sécurisé de bout en bout
* Ne transite pas par Internet et permet une gestion **centralisée** et **simplifiée** du réseau via un opérateur qui supervise et maintient l’ensemble.
* Intérêts
	* Garantir une QoS excellente
	* Optimise et fluidifie le trafic réseau
	* Mise en place facile VPN grande échelle
* Permet de perdre moins de temps à chercher dans les tables de routage
* Acheminement des paquets par ***commutation*** plutôt que par routage
* ***3 types*** de routeurs
	* ***CE (Customer Edge)*** 
		* En dehors du nuage MPLS
		* Installé chez le client
		* Permet au client de se connecter au réseau MPLS
	* ***PE (Provider Edge)***
		* Dans le nuage MPLS, situé au extrémité
		* Routeur d'entrée chez l'opérateur au réseau MPLS
		* Labelise en entrée du réseau les paquets IP reçu du CE
		* Delabelise en sortie du réseau les paquets IP reçu par P
	* ***P (Provider)***
		* Situé au centre du nuage MPLS
		* Commute les paquets labelisés grâce à sa table de label LIB (Label Information Base)
		* Il modifie le label de sortie en fonction de label d'entrée à partir de sa table de label ()

# 2) Système d'étiquetage des paquets (flag)
* Utilise un ***système d'étiquetage*** (flag) des paquets afin de ***séparer différents type de trafic*** au sein du réseau MPLS
	* Voix
	* Video
	* Email
	* VPN
* Les routeurs regarde le ***label de destination*** afin de savoir quel chemin prendre
* Le label est choisi par rapport à la ***destination du paquet***

## 1) Entrée du paquet dans le réseau MPLS
* 1) ***Paquet IP qui arrive au porte du nuage MPLS*** via routeur ***PE***
* 2) Le ***protocole routage IP du paquet*** est déterminé
* 3) A partir de l ****'IP de destination du paquet***, on ***assigne un label*** ainsi que le ***routeur du prochain saut*** afin d'atteindre la destination
* 4) Ajout de ***l'en-tête(label)***
* 5) Envoi vers le ***nœud suivant*** (routeur)
* Exemple table FEC sur routeur PE
```
InPort | IP_DESTI | Label_attribué | Saut_routeur
   P1   193.48.1.35       L5             P2
```

## 2) Commutation du paquet étiqueté dans le réseau MPLS via routeur P
* 1) Paquet IP étiqueté arrive au routeur P ou LSR (pareil)
* 2) Protocole de routage déterminé à partir de la LIB (table de labels commun, partagé)
* 3) Modification de la valeur du label
* 4) Envoi du paquet avec étiquète vers routeur suivants
* Exemple table LIB sur routeur P
```
InPort | InLabel | Saut_routeur | OutLabel
  P1       L5           P3           L21
```

## 3) Arrivée du paquet à destination
* 1) Paquet IP labelisé arrive au routeur de sortie PE
* 2) Retrait du label et transmission du paquet IP dans le réseau de destination (LAN) au niveau de la couche réseau
![[MPLS_1.png]]
# 3) Glossaire
* ***LSR*** (Label Switch Router)
	* Routeur avec fonctionnalités MPLS
		* Routeurs de ***cœur LSR ***
			* Communication de labels
		* Routeurs de ***bordure Edge LSR***
			* Insèrent et suppriment un label
* ***Label*** : Champ de 2 octets (16bits) entre couche 2 et 3
* ***FEC*** (Forwarding Equivalent Class)
	* Groupe de paquets recevant le même label et suivant le même chemin
* ***LDP*** (Label Distribution Protocol)
	* UDP/TCP port ***646***
	* ***224.0.0.2***
	* Assure l'échange de label entre voisins
* ***LIB*** (Label Information Base)
	* Table de labels
	* Contient les labels locaux et ceux appris des voisins
* ***LFIB*** (Label forwarding information base)
	* Extraite de la LIB
	* Table de commutation
	* Label local | Label de sortie | Préfixe | Interface de sortie | Saut 

# 4) Commandes
* Activer MPLS
```
ip cef
```
* Assigner réseau MPLS à une interface
```
int G0/0
	mpls ip
```
* Afficher table de commutation (LFIB)
```
sh mpls forwarding-table
```
* Afficher table des labels (LIB)
```
sh mpls ldp bindings
```

# 5) VPN MPLS
* Permet de ***séparer flux VPN*** par utilisateur ou section
* Paquets MPLS constituer de ***2 labels*** (au lieu d'un comme précédemment)
	* ***Label extérieur*** : Identifie le chemin vers la destination comme précédemment, il est modifié lors de la commutation avec les Providers
	* ***Label intérieur*** : Spécifie le numéro du VPN attribué au client, il n'est jamais modifié
	* C'est le ***PE source*** qui applique ces deux labels au paquet lorsqu'un VPN MPLS est utilisé
* ***Routeur CE*** (Customer Edge) = Routeur externe client relie à un PE
	* Ne connait pas MPLS 
	* Ne connait pas l'existence du VPN
* ***Routeurs PE*** (Provider Edge) = Routeurs bordure nuage
	* Echange les informations de routage avec le CE 
	* Isole chaque CE par une VRF ! 
	* Transforme les routes apprises par le CE en routes VPNv4 
	* Echange par MP-BGP des routes VPNv4 avec les autres PE
* ***Routeur P*** (Provider) = Routeur interne dans nuage
	* Transmet les informations de PE1 à PE2 
	* Routeur interne au système autonome
* ***Route distinguisher***
	* Identifie le client comme la VRF
* ***Route target***
	* Valeur pour spécifier à chaque route target un VPN
## Commandes 
* Configurer le route distinguisher sur un PE
```
ip vrf @nom_vrf
	RD @ASN:@numero_x_x
```
* Configurer le route target sur un PE
```
ip vrf @nom_vrf
	#Exportation des routes
	route-target import ASN:nn1
	#Importation des routes
	route-target export ASN:nn2
	#Importation et exportation simulatanée
	route-target both ASN:nn
```