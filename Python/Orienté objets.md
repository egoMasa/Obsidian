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

* Ajouter paramètres facultatifs `*args` (sac à paramètres potentiels)
```python
class Personne
	def __init__(self,nom,prenom,age,*args):
		self.nom = nom
		self.prenom = prenom
		self.age = age
		if 'masculin' in args:
			self.sexe = 'masculin'
		if 'etudiant' in args:
			self.statut = 'etudiant'
instance = Personne("fournier","jérémy",19,etudiant)
print(instance.__dict__)
>>{'nom': 'fournier', 'prenom': "jeremy", 'age':19, 'statut':'etudiant'}
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

* Accesseurs ``get_XXXX()`` et Mutateurs ``set__XXXX()``
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

* Afficher attributs de classe avec `__dict__` et méthodes `dir`
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

## User INPUT et OUTPUT
* Ajouter méthode `get_user_inputs(self)`
```python
class Monstre:

	def __init__(self,nom,vie):
		self.__nom = nom
		self.__vie = vie

	def get_user_input(self)
		while True:
			try:
				nom = input("Entrez nom : ")
				vie = int(input("Entrez vie : "))
				return self(nom, vie)
```
## Paramètres facultatifs
* Ajouter paramètres facultatifs `*args` (sac à paramètres potentiels)
```python
class Personne
	def __init__(self,nom,prenom,age,*args):
		self.nom = nom
		self.prenom = prenom
		self.age = age
		if 'masculin' in args:
			self.sexe = 'masculin'
		if 'etudiant' in args:
			self.statut = 'etudiant'
instance = Personne("fournier","jérémy",19,etudiant)
print(instance.__dict__)
>>{'nom': 'fournier', 'prenom': "jeremy", 'age':19, 'statut':'etudiant'}
```

## Heritage de classe
```python
class Parent:
	def __init__(self,nom):
		self.nom = nom

class Enfant(Parent):
	def __init__(self,nom,prenom):#Init Enfant 
		super().__init__(self,nom)#Init Parent
		self.prenom = prenom
		
enfant = Enfant("Nom","Prenom")
```