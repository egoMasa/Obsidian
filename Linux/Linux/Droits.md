# Guide Droits Linux

## I) Différents droits

* r = lire
* w = ecrire (modifier, vider)
* x = executer (script, programme)

* Afficher droits d'un fichier/repertoire 
```
(machine)root#ls -l
```

![[Pasted image 20220902170400.png]]

* 7 = r,w,x
* 6 = r,w
* 5 = r,x
* 4 = r
* 3 = w,x
* 2 = w
* 1 = x
## I) Commandes

* Modifier les droits
```
(machine)root#chmod XXX @fichier/repertoire
```

* Modifier propriétaire 
```
(machine)root#chown @user @fichier
ou
(machine)root#chown -R @user @repertoire
ou
(machine)root#chown @user:@groupe @fichier
```

