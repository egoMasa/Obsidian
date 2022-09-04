# Guide format YAML python
## I) Syntaxe générale 
![[Pasted image 20220828131908.png]]
 ![[Pasted image 20220831122236.png]]

## II) Module de traitement : ***yaml***
* Importer module
<code>import yaml </code>

## III) Méthodes
* Charger fichier YAML format dictionnaire
```python
with open ("fichier.yaml","r") as fd :
	doc = yaml.safe_load(fd)
	ou
	doc = yaml.safe_load_all(fd)
```

* Transformer dictionnaire en format YAML

```python
document = [
			{"clef":"valeur", "clef2":"valeur2"}
			
]
print(yaml.dump(document))

```

* Enregistrer nouvelles données dans fichier
```python
données = [
			{"clef":"valeur", "clef2":"valeur2"}
			
]
with open ("fichier.yaml","w+") as fd :
	yaml.dump(données, fd)
```