# RIP (petit réseau)

### Versions :

* V1 envoie ses messages en ***BROADCAST*** et ne gère aucun sous-réseau

* V2 envoie ses messages en ***MULTICAST*** et gère les sous réseau (VLSM)

### Commandes RIP :

-   Activer RIP sur le routeur
```bash
R1(config)#router rip
```

-   Annoncer un réseau voisin
``` bash
R1(config-router)#network @ip_reseau_voisin 
```

-   Changer version de RIP
```bash
R1(config-router)#version 2 
```

-   Désactiver résumé automatique des routes
```bash
R1(config-router)#no auto-summary
```

-   Partager route par défaut
```bash
R1(config-router)#default-information originate
```

-   Empêcher interface de partager mise à jour de la table
```bash
R1(config-router)#passive-interface @interface
```

# IPV6 
* Groupe de multicast : ***FF02::9***
```cisco
ipv6 unicast routing
ipv6 router rip @nom

int @int
	ipv6 adress 2001:db8:cafe::1/64
	ipv6 rip @nom enable
	ipv6 rip NOM summary-address PREFIX/MASQUE
	
sh ipv6 route rip
sh ipv6 rip protocols
sh ipv6 int brief

```