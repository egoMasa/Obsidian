# Configuration de base
* De base tous les sw sont sur le meme VLAN 1 natif
* Afficher VLANs et ports reliés 
```
Switch#show vlan brief
```
* Afficher un résumé sur les VLAN.
```
S1#show vlan summary
```
## Partie 1 : Création de VLAN
* Créez un VLAN avec un numéro d'identité valide.
```
Switch(config)#vlan _vlan-id_
```
* Indiquez un nom unique pour identifier le VLAN.
```
Switch(config-vlan)#name _vlan-name_
```
## Partie 2 : Attribution de port VLAN
* Passer en mode de configuration d'interface.
```
Switch(config)#interface _interface-id_
```
* Définissez le port en mode d'accès.
```
Switch(config-if)#switchport mode access
```
* Affecter le port à un réseau local virtuel.
```
Switch(config-if)#switchport access vlan _vlan-id_
```
* Définir l'état de confiance d'une interface, et pour indiquer quels champs du paquet sont utilisés pour classer le trafic.
```
S3(config-if)#mls qos trust [cos | device cisco-phone | dscp | ip-precedence]
```
## Partie 3 : Configuration du trunk
* Passer en mode de configuration d'interface.
```
Switch(config)#interface _interface-id_
```
* Réglez le port en mode de trunking permanent.
```
Switch(config-if)#switchport mode trunk
```
* Choisissez un VLAN natif autre que le VLAN 1
```
Switch(config-if)#switchport trunk native vlan _vlan-id_
```
* Indiquer la liste des VLAN autorisés sur la liaison trunk.
```
Switch(config-if)#switchport trunk allowed vlan _vlan-list_
```
* Exemple
```
S1(config)#interface fastEthernet 0/1
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 99 
S1(config-if)#switchport trunk allowed vlan 10,20,30,99 
S1(config-if)#end
```
* Reinitialiser trunk par défaut
```
S1(config)#interface fa0/1 
S1(config-if)#no switchport trunk allowed vlan 
S1(config-if)#no switchport trunk native vlan 
S1(config-if)#end
```

# Routage inter-vlan

## Methode 1 : Router on stick

### Etape 1 : Créez et nommez vlan
```
S1(config)#vlan 10
S1(config-vlan)#name LAN10
S1(config-vlan)#exit
S1(config)#vlan 20
S1(config-vlan)#name LAN20
S1(config-vlan)#exit
```
### Etape 2 : Créez interface de gestion
```
S1(config)#interface vlan 99
S1(config-if)#ip add 192.168.99.2 255.255.255.0 
S1(config-if)#no shut
S1(config-if)#exit
S1(config)#ip default-gateway 192.168.99.1 
```
### Etape 3 : Configurer ports d'accès
```
S1(config)#interface fa0/6
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 10
S1(config-if)#no shut
S1(config-if)#exit
```
### Etape 4 : Configurer ports trunk
```
S1(config)#interface fa0/1
S1(config-if)#switchport mode trunk
S1(config-if)#no shut
S1(config)#interface fa0/5
S1(config-if)#switchport mode trunk
S1(config-if)#no shut
```
### Etape 5 : Configurer routeur
```
R1(config)#interface G0/0/1.10
R1(config-subif)#description Default Gateway for VLAN 10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip add 192.168.10.1 255.255.255.0
R1(config-subif)#exit
R1(config)#interface G0/0/1.20
R1(config-subif)#description Default Gateway for VLAN 20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip add 192.168.20.1 255.255.255.0 R1(config)#interface G0/0/1.99
R1(config-subif)#description Default Gateway for VLAN 99
R1(config-subif)#encapsulation dot1Q 99 
R1(config-subif)#ip add 192.168.99.1 255.255.255.0 R1(config)#interface G0/0/1
R1(config-if)#description Trunk link to S1 R1(config-if)#no shut
```
* Vérifier configuration
```
show ip route
show ip interface brief
show interfaces
show interfaces trunk
```

## Methode 2 : SW3
### Etape 1 : Créez et nommez vlan
```
D1(config)#vlan 10
D1(config-vlan)#name LAN10
D1(config-vlan)#vlan 20
D1(config-vlan)#name LAN20
D1(config-vlan)#exit
```
### Etape 2 : Créez interface VLAN SVI
```
D1(config)#interface vlan 10
D1(config-if)#description Default Gateway SVI for 192.168.10.0/24
D1(config-if)#ip add 192.168.10.1 255.255.255.0 D1(config-if)#no shut
D1(config-if)#exit
D1(config)#int vlan 20
D1(config-if)#description Default Gateway SVI for 192.168.20.0/24
D1(config-if)#ip add 192.168.20.1 255.255.255.0 D1(config-if)#no shut
```
### Etape 3 : Configurez ports d'accès
```
D1(config)#interface GigabiteThernet1/0/6
D1(config-if)#switchport mode access
D1(config-if)#switchport access vlan 10
D1(config)#interface GigabiteThernet1/0/18 D1(config-if)#switchport mode access
D1(config-if)#switchport access vlan 20
D1(config-if)#exit
```
### Etape 4 : Activer routage IP
```
D1(config)#ip routing
```

## Methode 3 : SW3 avec Routeur externe
### Etape 1 : Configurez le port routé
```
D1(config)#interface GigabitEthernet1/0/1
D1(config-if)#description routed Port Link to R1
D1(config-if)#no switchport
D1(config-if)#ip address 10.10.10.2 255.255.255.0 D1(config-if)#no shut
```
### Etape 2 : Activer routage IP
```
D1(config)#ip routing
```
### Etape 3 : Configurez le routage
```
D1(config)#router ospf 10
D1(config-router)#network 192.168.10.0 0.0.0.255 area 0
D1(config-router)#network 192.168.20.0 0.0.0.255 area 0
D1(config-router)#network 10.10.10.0 0.0.0.3 area 0
```


