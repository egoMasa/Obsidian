# Guide MPLS

# 1) Généralités
* MPLS = ***Multiprotocol Label Switching***
* Intérêts
	* Qualité de service
	* Gestion trafic et répartition de charge
	* Mise en place facile VPN grande échelle
* Assure la ***commutation de paquets IP***
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
# 2) Commandes
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

# VPN MPLS
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