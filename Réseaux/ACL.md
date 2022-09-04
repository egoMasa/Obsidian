# Guide ACL

## I) Principe et explication

* Une ACL est une liste de règles qui va être mise en place sur un réseau. Son but est de filtrer les paquets qui ont le droit de circuler ou non.  Elle intervient sur les couches Transport (UDP/TCP) et Réseau(IP).

❗On peut définir une seule ACL dans un sens par interface.

* Il existe deux types d’ACL : 

	-   ***ACL Standard*** = Placé près de destination, permet d’analyser le trafic qu’à partir de l’adresse IP source, donc faible précision. Elles sont numérotées de 1-99 et 1300-1999
    
	-   ***ACL Étendues*** = Placé près de source, permet d’analyser le trafic en fonction de plusieurs critères, donc plus grande précision. Elles sont numérotées de 100-199 et 2000-2699
    

* Elles permettent deux actions : ***permit/deny***. Selon nos besoins de filtrage on renseigne ce dont on a besoin. Exemple : Empêcher les requêtes TCP d’une source précise ou inversement.

## II) Commandes ACL standard

1.  ***Création ACL***
```bash
R1(config)#access-list @numero @permit|deny @ip_reseau @masque_invert  
R1(config)#access-list @numero permit any
```

❗Il faut renseigner le fait que tous les autres traffic puissent passer malgré les ACL car par défaut il bloque tout le reste.

2.  ***Activation sur interface***
```bash
R1(config)#interface @interface  
R1(config-if)#ip access-group @num @in|out
```

## III) Commandes ACL Etendue

1.  ***Création ACL***
```bash
R1(config)#access-list @numero @permit|deny @type @ip_source @masque_invert @ip_desti @masque_invert  
R1(config)#access-list @numero permit any any
```
ou
```bash
R1(config)#access-list @numero @permit|deny @type host @ip host @ip  
R1(config)#access-list @numero permit any any
```

2.  ***Activation sur interface***
```bash
R1(config)#interface @interface  
R1(config-if)#ip access-group @num @in|out
```

## IV) Commandes ACL Etendue nommée

1.  ***Création ACL***
```bash
R1(config)#ip access-list @extended|standard @nom  
			permit ...  
			deny ... any any
```

2.  ***Activation sur interface***
```bash
R1(config)#interface @interface  
R1(config-if)#ip access-group @num @in|out
```