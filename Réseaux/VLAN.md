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

-   Créer un VLAN
```bash
R1(config)#vlan @numero  
R1(config-vlan)#name @nom
```

-   Afficher VLAN et ses affectations
```bash
R1#show vlan brief
```

-   Affecter un VLAN à une interface mode access
```bash
R1(config)#interface range fastEthernet 0/5-8  
R1(config-if)#switchport mode access
R1(config-if)#switchport access vlan @num
```

-   Affecter un VLAN à une interface mode trunk
```bash
R1(config)#interface @interface  
R1(config-if)#switchport trunk encapsulation dot1q
R1(config-if)#switchport mode trunk
```
