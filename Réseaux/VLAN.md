# Guide VLAN

## I) Principe et explication

* Les VLAN sont un mécanisme qui permet de splitter les serveurs en plusieurs parties distinctes même sur un câble commun. On assigne à chaque VLAN (une sorte de classe) un ensemble de machines au choix. Le but de cette fonctionnalité est de réduire la taille des domaines de diffusion d’informations et de regrouper de façon logique certains groupes d’utilisateurs.

![[Pasted image 20220901124817.png]]

❗Il y a plusieurs étapes à suivre pour mettre en place un VLAN :

1.  ***Ajouter le VLAN*** et définir son nom
    
2.  ***Affecter un port/interface*** à un vlan
    

❗Un port sur un switch peut avoir plusieurs rôle :

1.  ***Port d’accès*** = utilisé pour la connexion terminale d'un périphérique (pc, imprimante, serveur, ...) appartenant à un seul vlan
    
2.  ***Port Trunk***= utilisé dans le cas ou plusieurs vlans doivent circuler sur un même lien. C'est par exemple le cas de la liaison entre deux switchs ou bien le cas d'un serveur ayant une interface appartenant à plusieurs vlans.

![[Pasted image 20220901125004.png]]

## II) Commandes VLAN

### Partie 1 : Ajout VLAN aux interfaces avec mode correspondant
1. Créer un VLAN
```bash
SW(config)#vlan @numero  
SW(config-vlan)#name @nom
```

-   Afficher VLAN et ses affectations
```bash
SW#show vlan brief
```

2. Affecter VLAN aux interfaces
```bash
SW(config)#interface range fastEthernet 0/0-5  
SW(config-if)#switchport mode access
SW(config-if)#switchport access vlan @num
```

3. Affecter mode trunk à une interface (relié au routeur)
```bash
SW(config)#interface @interface  
SW(config-if)#switchport mode trunk 
```

### Partie 2 : Configuration adressage IP

1. Création sous interface via encapsulation dot1Q
```
R1(config)#interface fa0/1.10 #Au choix 
R1(config-subif)#encapsulation dot1Q @num_vlan
R1(config-subif)#ip address 172.17.@num.1 255.255.255.0 

R1(config-subif)#interface fa0/1.30 
R1(config-subif)#encapsulation dot1Q @num_vlan2
R1(config-subif)#ip address 172.17.@num.1 255.255.255.0
```
