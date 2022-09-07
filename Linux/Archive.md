# Guide archive Linux

## I) Principe et précision
* Sous linux les archives sont au format ***.tar*** mais ne sont pas compressées
* Plusieurs logiciels de compression : ***gzip***
* Une archive compressée est au format : ***.tar.gz***
* 
## I) Commandes
* Compresser fichier/dossier
```
(machine)root#tar -cvzf @nom_final.tar.gz @chemin_cible
```
-c = Créer une archive
-z = Compresser l'archive via gzip
-v = Verbeux, affiche progression
-f = Specifier nom archive finale

* Décompresser fichier/dossier
```
(machine)root#tar -xvzf @archive.tar.gz
```
-x = Décompresser