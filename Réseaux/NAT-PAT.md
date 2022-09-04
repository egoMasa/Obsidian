# Guide NAT/PAT

## I) NAT (Network Address Translation)

### Principe et explications :

* Le NAT est un mécanisme extrêmement répandu est qu’on utilise tous lorsqu’on se connecte à un réseau. Le principe est de définir une adresse IP publique et privée, ainsi à l’intérieur d’un réseau les IP seront privés et quand une requête sortira elle sera modifiée en IP Publique. C’est le routeur qui joue ce rôle de passer d’une IP privée à publique lorsque l’on sort de notre réseau local.

* Ce mécanisme à été mis en place afin de répondre au manque d'adresses IPV4 disponible. On a tous les mêmes adresses privées dans nos réseaux personnels, généralement 192.168.1.X.

* Par exemple, lors d’une requête vers un site web extérieur, le paquet de notre ordinateur va passer par notre routeur qui est notre passerelle. Elle va modifier notre paquet et va remplacer l’adresse IP source de notre PC “192.168.1.10” en “61.61.61.61” qui est l’adresse publique de notre routeur

![[Pasted image 20220831230804.png]]

* Lors de la phase retour, le serveur web va envoyer sa réponse à l’adresse publique du routeur “61.61.61.61”. Ensuite le routeur qui aura mémoriser cette échange dans une “table de translations” va transférer le paquet vers l’ordinateur

![[Pasted image 20220831230909.png]]
### Types de NAT :

-   NAT statique = un pour un, un IP publique et un IP privé par appareil, les IP sont fixes et ne bougent jamais. Simple mais demande pas mal d’IP et montre des faiblesses

![](https://lh4.googleusercontent.com/TRPjh4fHzl-V07ahtsLsFAS133_FRRayAQmVsnbT0-S_eUBhGuUVP8TNdCjfYLVvuC6eoRefqXI8B8cn9NfV_CUwsOcR0YLlamNj4tY2HCNGxNNUV3eAEGqmfPxl5xYxGmzSOIQ-2zZf2kQIiAtsZVrQZ1anCIAzEZFaClXqllaKw9heQAOJSG5x-Q)

-   NAT dynamique = Une IP publique et privée mais elles sont interchangeables, si une IP publique est disponible alors n’importe quelle machine peut l’utiliser. Une sorte de location temporaire d’IP.

![[Pasted image 20220831231145.png]]

### Commandes NAT Statique

```bash
R1(config)#interface @interface  
R1(config-if)#ip nat inside

R1(config)#interface @interface  
R1(config-if)#ip nat outside

R1(config)#ip nat inside source static @ip_reseau_interne @ip_exterieur
```

### Commandes NAT dynamique :

```bash
R1(config)#interface @interface  
R1(config-if)#ip nat inside

R1(config)#interface @interface  
R1(config-if)#ip nat outside

R1(config)#ip nat pool @nom @ip_debut @ip_fin netmask @masque  
R1(config)#access-list 1 permit @ip_interne @masque  
R1(config)#ip nat inside source liste 1 pool @nom
```


## II) PAT (Port Address Translation)

### Principe et explications :
* Le PAT est une forme du NAT dynamique avec la particularité d’ajouter un système de numéro de port. De ce fait, une IP publique suffit et seul le numéro de port importe pour avoir une connexion complète.

![[Pasted image 20220831231626.png]]

* Une notion unique de PAT est la redirection de port, son principe consiste à définir une IP publique du routeur d’un réseau avec un numéro de port à un service interne utilisant une autre IP. En résumé le port ne change mais l’IP de destination OUI

![[Pasted image 20220831231723.png]]

### Commandes PAT statique:

```bash
R1(config)#interface @interface  
R1(config-if)#ip nat inside

R1(config)#interface @interface  
R1(config-if)#ip nat outside

R1(config)#ip nat inside source static @tcp/udp @ip_interne @port_interne @ip_externe @port_externe 
```

### Commandes PAT dynamique sur une adresse:

```bash
R1(config)#interface @interface  
R1(config-if)#ip nat inside

R1(config)#interface @interface  
R1(config-if)#ip nat outside

R1(config)#access-list 1 permit @ip_interne @masque_invert
R1(config)#ip nat inside source list 1 interface @interface overload
```

### Commandes PAT dynamique sur pool d’adresses:

```bash
R1(config)#interface @interface  
R1(config-if)#ip nat inside

R1(config)#interface @interface  
R1(config-if)#ip nat outside

R1(config)#ip nat pool @nom @ip_debut @ip_fin netmask @masque
R1(config)#access-list 1 permit @ip_interne @masque_invert
R1(config)#ip nat inside source liste 1 pool @nom overload
```
