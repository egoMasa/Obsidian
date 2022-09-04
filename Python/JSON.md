# Guide format JSON python
## Syntaxe générale 
![[Pasted image 20220828134841.png]]
  

## Module de traitement 
* Importer module
<code>import json</code>

## Méthodes
* Charger fichier JSON
```python
with open ("fichier.json","r") as fd :
	json.load(fd)
```
* Ajouter données (dico) dans fichier JSON
```python
données = {...}
with open ("fichier.json","w+") as fd :
	json.dump(données, fd)
```
