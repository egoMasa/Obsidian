# Guide pour les dictionnaires python
## Syntaxe générale 
<code>dico  = {"couleur":"rouge","marque":"renault"}</code>
## Méthodes 
* Récupérer toutes les cléfs d'un dico 
```python
dico.keys()
```
* Récupérer toutes les valeurs d'un dico 
```python
dico.values()
```
* Récupérer toutes les cléfs et valeurs d'un dico 
```python
dico.items()
```
* Récupérer toutes les valeurs d'un dico 
```python
dico.values()
```
* Récupérer la valeur d'une clef spécifique d'un dico 
```python
dico[clef]
dico.get("clef")
```
* Ajouter un dictionnaire avec un autre : dico.update()
```python
dico_1 = {"nom":"Sublet-Vial","prenom":"Stella"}
dico_2 = {"age":"19","ville":"Marseille"}
dico_1.update(dico_2)
>> dico1 = "nom":"Veran","prenom":"Olivier","age":"160","ville":"Uranus"
```
* Afficher nombre de clefs d'un dictionnaire
```python
print(len(dico))
```
* Ajouter un clef et sa valeur dans un dico existant
```python
dico["new_key"]="new_value"
```
* Modifier valeurs d'une clef
```python
dico_1["new_key"]="new_value_updated"
```
* Lister clefs via boucle
```python
for clef in dico.keys():
        print(clef)
for clef in dico:
        print(clef)
```
* Lister valeurs via boucle
```python
for valeur in dico.values():
        print(valeur)
```
* Lister clefs et valeurs en même temps via boucle
```python
for clef, valeur in dico_1.items():
        print(clef,valeur)
```
 * Supprimer clef d'un dictionnaire
```python
del dico["clef"]
```
* Copier un dictionnaire
```python
dico2.copy(dico1)
```
