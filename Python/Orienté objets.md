# Guide programmation orienté objet python

## I) Principe 

*  La programmation orienté objet est un style de programmation opposé à la procédurale où les variables et fonctions seront définis dans un bloc parent nommé class. Créer une classe c'est aussi créer un nouveau type unique (comme str,int,...) L'appel de cette classe créera un objet l'on pourra manipuler

* Les ***fonctions*** sont des ***methodes***

* Les ***variables*** sont des ***attributs***

* Un ***objet crée*** est une ***instance de classe***

* Certaines classes peuvent hérités de certains attribut d'autres 
![[Pasted image 20220831184041.png]]

## II) Syntaxe 
* Déclarer une class avec attributs et méthodes
```python
class Monstre:

    def __init__(self) -> None:
        self.vie = 100
        self.taille = 200

    def attack(self,degats):
        self.vie -=degats
        print("Degats infligés de -",degats)

objet = Monstre() #On créer l'objet/instance

print("Vie initiale: ",objet.vie) # On affiche les attributs de la classe
>> 100

objet.attack(20) #On appel la methode attack()

print("Vie finale",objet.vie)
>>80
```

* Afficher contenu d'une classe
```python
objet = Monstre()
print(objet.__doc__)#Affiche contenu de la classe
```
* Dépendance de classes 
```python
class Monstre:

    def __init__(self,soif) -> None:
        self.soif = soif
        
class Stats:

	def water(self)
		print("100%")

objet = Monstre(soif = Stats().water)
print(objet.soif)
>>100%

```
* Créer système d'heritage entre deux classes 
```python
class Monstre:

    vie = 100
    taille = 200

    def attack(self,degats):
        self.vie -=degats
        print("Degats infligés de -",degats)

class Shark(Monstre): # Shark herite des variables et fonctions de Monstre() : vie, dégats

    def __init__(self,speed) -> None:
        self.speed = speed

objet = Shark(600)

print(objet.vie)
>> 100
```

* Verifier si une objet possède un attribut précis 
```python
objet = Monstre()
hasattr(objet,"attribut") = {True,False}
```

* Ajouter un attribut avec sa valeur à une class
```python
objet = Monstre()
objet.dents = 21
ou
setattr(objet,"dents",21)
```

* ``*args`` et `**kwargs`
```python
def __init__ (self,taille,*args)
#Attribut optionnel ne faisant pas partie de la structure de l'objet
if "exemple" in args
	self.attribut = ...


#Modifier structure de notre classe
def __init__ (self,taille,**kwargs):
	if 'exemple' in kwargs:
		blabla
```

* Attributs privés (rendre impossible l'action directe, faut passer par des méthodes qui joue le role d'interface)
```python
class Monstre:
	def __init__(self,a):
		self.__nom = a

objet = Monstre("Shark")
print(objet.nom)
>> Attribute Error # On peut plus y accéder directement
```

* Accesseurs et Mutateurs ``get_XXXX()`` et ``set__XXXX()`` (méthodes d'accès aux attributs privés)
```python
class Monstre:

	def __init__(self,a):
		self.__nom = a
		
	def get_nom(self): #Obtenir l'attribut
		return self.__nom

	def set_nom(self,x): #Modifier l'attribut
		self.__nom = x

objet = Monstre("Shark")

objet.get_nom()
>>Shark

objet.set_nom("Kraken")
objet.get_nom()
>>Kraken
```

* Fonctions `dir` et `__dict__`
```python
class Monstre:

	def __init__(self,a,b):
		self.nom = a
		self.vie = b
		
	def attack(self):
		print("Attack")

objet = Monstre("Shark",100)

print(objet.__dict__) # Affiche Attributs d'une classe
>>{'nom': 'Shark', 'vie': 100}

print(dir(Monstre)) # Affiche Méthodes d'une classe
>>['attack']

print(dir(objet)) # Affiche méthodes et attributs de l'instance
>>['attack','nom','vie']
```

* Input et Output
```python
class Monstre:

	def __init__(self,a,b):
		self.__nom = a
		self.__vie = b
		
	def get_nom(self):
		return self.__nom
		
	def get_vie(self):
		return self.__vie
		
	def set_nom(self,x):
		self.__nom = x
		
	def set_vie(self,y):
		self.__vie = y

	def get_user_input(Monstre)
		Monstre.set_nom(input("Saisir nom :"))
		Monstre.set_vie(int(input("Saisir vie : ")))
		
objet = Monstre(0,0)

objet.get_user_input()
>>"Saisir nom : " Shark
>>"Saisir vie : " 100

print(objet.get_nom())
>>"Shark"

print(objet.get_vie())
>>100
```

* Décorateurs de classe
```python

```