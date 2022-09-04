# Guide programmation orienté objet python

## I) Principe 

*  La programmation orienté objet est un style de programmation où les variables et fonctions seront définis dans un bloc parent nommé class. L'appel de cette classe créera un objet l'on pourra manipuler

* Les ***fonctions*** sont des ***methodes***

* Les ***variables*** sont des ***attributs***

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

objet = Monstre() #On créer l'objet

print("Vie initiale: ",objet.vie) # On affiche les attributs de la classe
>> 100

objet.attack(20) #On appel la methode attack()

print("Vie finale",objet.vie)
>>80
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

setattr(objet,"new_attrib","valeur")
ou
objet.newttrib = "valeur"

```