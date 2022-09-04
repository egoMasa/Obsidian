# Guide pour les fichiers python
## Syntaxe générale 
```
with open ("fichier","{r,w,x}",encoding = "?") as fd :
	instructions....
```
* r = read
* w = write
* x = execute
## Méthodes 
* Ouvrir fichier
<code>with open ("fichier","{r,w,x}",encoding = "?") as fd :</code>

* Lire une ligne
<code>fd.readline()</code>

* Lire tout le fichier sous format ***liste***
<code>fd.readlines()</code>

* Lire tout le fichier sous format ***str***
<code>fd.read()</code>

* Ecrire dans un fichier
<code>fd.write()</code>