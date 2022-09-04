# Guide Routage statique

## I) Principe et explication

* Sur un petit réseau avec seulement quelques équipements réseau, il est souvent plus simple de mettre en place un système de routage statique. Son principe consiste à annoncer chaque réseau manuellement au routeur qui va recevoir le paquet, on appelle cela mettre en place une route statique vers un réseau.

* La route par défaut quant à elle est utilisée pour indiquer une route vers un réseau dont on ne connaît pas son emplacement exact annoncé via les routes statique, par exemple un site internet (une sorte de sortie de secours)

❗Pour qu’un routage statique fonctionne, il faut impérativement respecter plusieurs règles d’or:

1.  La passerelle (routeur) de votre PC doit connaître la route pour atteindre le réseau voulu
    
2.  La passerelle (routeur) de la destination doit connaître la route vers le réseau de l’envoyeur
    
![[Pasted image 20220831224008.png]]
## II) Commandes route statique

-   Ajouter une route statique
```bash
R1(config)#ip route @réseau_desti @masque_desti @ip_saut_suivant`
```

-   Afficher table de routage (les routes)
```bash
R1#sh ip route`
```

-   Ajouter une route statique par défaut
```bash
R1(config)#ip route 0.0.0.0 0.0.0.0 @ip_saut_suivant`
```
