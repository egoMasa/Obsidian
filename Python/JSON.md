# Guide format JSON python
## Syntaxe générale 
![[Pasted image 20220828134841.png]]
  

## Module de traitement 
* Importer module
<code>import json</code>

## Méthodes
* Charger fichier JSON (json->python)
```python
with open ("fichier.json","r") as fd :
	json.load(fd) #Désérialisation en str
	json.loads(fd) #Désérialisation en dico
```
* Ajouter données (dico) dans fichier JSON (python->json)
```python
données = {...}
with open ("fichier.json","w+") as fd :
	json.dump(données, fd) #Sérialisation en str
	json.dumps(données, fd) #Sérialisation en dico
```

* Insérer JSON dans SQLite
```python
import sqlite3
sqlite3.connect("db_json")
json.load(open('file.json','r'))
create_query = "CREATE TABLE IF NOT EXISTS (ID INTEGER PRIMARY KEY, JSON VARCHAR(1000))"
insert_query = "INSERT INTO VOITURE"
```
* Exemple de fichier JSON
```json
{"details":[
	{"marque":"Peugeot",
	"modele":404
	"couleur":"vert"
	"annee":1970
	},
	{"marque":"Renault",
	"modele":5
	"couleur":"rouge"
	"annee":1990
	}
]}
```
* Manipuler fichier JSON sous python
```python
import json

with open("fichier.json","r") as fd
	fichier = json.load(fd)
	donnees = fichier["details"]
	for i in donnees:
		keys = i.keys()
		print(keys)
		values = i.values()
		print(values)
```