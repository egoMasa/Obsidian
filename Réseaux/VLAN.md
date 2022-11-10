# Guide VLAN

## I) Principe et explication

* Les VLAN sont un mécanisme qui permet de segmenter les serveurs en plusieurs parties distinctes même sur un câble commun. On assigne à chaque VLAN (une sorte de classe) un ensemble de machines au choix. Le but de cette fonctionnalité est de réduire la taille des domaines de diffusion d’informations et de regrouper de façon logique certains groupes d’utilisateurs.

❗Il y a plusieurs étapes à suivre pour mettre en place un VLAN :
1.  ***Ajouter le VLAN*** et définir son nom
2.  ***Affecter un port/interface*** à un vlan

❗Un port sur un switch peut avoir plusieurs rôle :
1.  ***Port d’accès*** = utilisé pour la connexion terminale d'un périphérique (pc, imprimante, serveur, ...) appartenant à un seul vlan
2.  ***Port Trunk***= utilisé dans le cas ou plusieurs vlans doivent circuler sur un même lien. C'est par exemple le cas de la liaison entre deux switchs ou bien le cas d'un serveur ayant une interface appartenant à plusieurs vlans.

## II) Switchs L2 (avec routage on stick (router externe))

### Partie 1 : Création VLAN, Affectation ports aux VLAN, Aggrégation mode Trunk
1. Créer un VLAN
```bash
SW(config)#vlan @numero  
SW(config-vlan)#name @nom
```
-   Afficher VLAN et ses affectations
```bash
SW#show vlan brief
```
2. Affecter VLAN aux interfaces en mode access
```bash
SW(config)#interface range fastEthernet 0/0-5  
SW(config-if)#switchport mode access
SW(config-if)#switchport access vlan @num
```
3. Affecter mode trunk à une interface
```bash
SW(config)#interface @interface  
SW(config-if)#switchport mode trunk
ou
SW(config-if)#switchport trunk allowed vlan XX,XX,XX
```

### Partie 2 : Routage inter-vlan (routeur passerelle)
* On configure seulement une interface du routeur mais on ajoute une sous interface pour chaque VLAN sur le réseau
* Affecter port trunk à l'interface du switch coté routeur
```bash
SW(config)#int @interface
SW(config-if)#switchport mode trunk
SW(config-if)#switchport trunk allowed vlan XX,XX,XX
```
* Créer sous interface vlan sur routeur 
```bash
R(config)#int G0/1.@num_vlan
R(config-if)#encapsulation dot1q @num_vlan
R(config-if)#ip add @ip_gw_vlanX @masque
#ip addr = L'interface du routeur avec le meme réseau que le vlan visé mais .1 car il va être la gw
```


## III) Switch LVL3 (sans router on-stick(switch = router externe))
### Partie 1 : Ajout VLAN aux interfaces avec mode correspondant
* Réinitialiser un switch (L2,L3)
```bash
SW#erase startup-config
SW#delete vlan.dat
SW#reload [save : no]
```
* Affecter port en mode Access (câble avec un seul VLAN dessus)
```bash
SW(config)#int @interface
SW(config-if)#switchport mode access
```
* Affecter VLAN à un port
```bash
SW(config)#int @interface
SW(config-if)#switchport access vlan XX
```
* Affecter port en mode trunk (câble avec plusieurs VLAN dessus)
```bash
SW(config)#int @interface
SW(config-if)#switchport trunk encapsulation dot1q
SW(config-if)#switchport mode trunk
```
* Configurer port en port routé
```bash
SW(config)#int @interface
SW(config-if)#no switchport
```

### Routage inter VLAN
 * Activer routage sur le SW L3
```bash
SW(config)#ip routing
```
 * Créer VLAN avec nom
 ```bash
SW(config)#vlan XX
SW(config-vlan)#name <nom>
```
 * Créer interface SVI (Switch virtual interface)
 ```bash
SW(config)#int vlan XX
SW(config-if)#ip address @ip @masque
```
 * Assigner VLAN à une interface physique (Fa/Ga)
 ```bash
SW(config)#int @interface
SW(config-if)#switchport mode access
SW(config-if)#switchport access vlan XX
```
 * Activer protocole OSPF avec le PID et le router ID et reseaux adjacents
  ```bash
SW(config)#router ospf @PID
SW(config-router)#router-id @id
SW(config-router)#network @reseau_adj @masque_invert
```

❗***A retenir*** :
*  Sur un SW L3 on utilise des ports virtuels (SVI) au lieu de vrai ports physique. Pourquoi ? Car on a plus besoins de passer par un router externe qui va router les VLAN, le SW L3 le fait à l'intérieur directement. Cela offre de meilleures performances et réduit le délai en plus du câblage en moins
* Sur un SW L3 pas default Gateway, c'est lui la Gateway désormais
* Sur un router on configure seulement OSPF et les interfaces de base maintenant

# Exemple configuration SW3
* 2 vlans
```
ip routing

no ip domain-lookup

interface FastEthernet0/1
	switchport access vlan 20
	switchport mode access

interface FastEthernet0/24
	switchport access vlan 51
	switchport mode access

interface Vlan20
	ip address 10.5.20.2 255.255.255.0

interface Vlan51
	ip address 10.5.51.2 255.255.255.0

router ospf 51
	router-id 4.5.5.11
	network 10.5.51.0 0.0.0.255 area 0
	network 10.5.20.0 0.0.0.255 area 0

line con 0
	logging synchronous
```