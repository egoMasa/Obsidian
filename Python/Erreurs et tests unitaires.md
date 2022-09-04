# Guide gestion erreurs/tests python

## I)Principe

* Quand on cherche à effectuer des tests, on utilise les termes ***try***, ***except***, ***else***, ***finally***

* Ils marchent comme une condition ***if*** mais ne prennent pas d'arguments.

* On peut verifier les type de certaines variables via le terme ***isinstance*** accompagné de ***assert***

## II) Syntaxe 
* Gestion erreurs
```python
try:
	print(1/0) #Impossible, erreur division par 0
	
except ZeroDivisionError: #On peut renseigner le type d'erreur
	print("Something Else")
	
else: #Si aucun expect correspond
	print("Else")

finally: #Dans touts les cas
	print("finally")
>>Something Else
>>Finally
```

* Erreurs customisées
```python
if condition:
	...
else:
	raise TypeError("Message about error")
```

* Verifications et tests unitaires
```python
string = "Hello world"
isinstance(string,str) = {True,False}
>>True

assert(isinstance(string,int))
>>AssertionError
```

