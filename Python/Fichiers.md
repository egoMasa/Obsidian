# Guide pour les fichiers python
## Syntaxe générale 
```python
with open ("fichier","{r,w,x}",encoding = "?") as fd :
	instructions...
```
* r = read
* w = write
* x = execute
## Méthodes 
* Ouvrir fichier
```python
with open ("fichier","{r,w,x}",encoding = "?") as fd :
```

* Lire une ligne
``fd.readline()``

* Lire tout le fichier sous format ***liste***
<code>fd.readlines()</code>

* Lire tout le fichier sous format ***str***
<code>fd.read()</code>

* Ecrire dans un fichier
<code>fd.write()</code>