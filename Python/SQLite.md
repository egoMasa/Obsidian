# Guide SQLite pour Python
* Importer sqlite3 dans python
```python
import sqlite3
```
* Connexion à la base de données
```python
connexion = sqlite3.connect('db_json')
```
* Préparer requête création de table
```python
create_query = 'create table if not exists voiture (id integer primary key, json varchar(1000))'
```
* Préparer requête insertion de donées
```python
insert_query = 'insert into voiture values (?,?)'
```
* Ouvrir table 
```python
table = connexion.cursor()
```
* Exécuter requêtes
```python
donnees = json.load(open('file.json','r'))
str_json_donnees = str(donnees)
table.execute(create_query)
table.execute(insert_query,(1,str_json_donnees))
```
* Afficher contenu d'une table
```python
table.execute('SELECT * FROM voiture')
mdata = table.fetchone()
print(mdata)
```
* Enregistrer changements et fermer connexion
```python
connexion.commit()
table.close()
```