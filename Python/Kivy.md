# Guide Kivy pour python

* C'est une librairie qui permet de créer une ***interface graphique*** d'une application avec python
* Kivy est compatible avec IOS/Android grace au moteur graphique ***OpenGL ES 2***

# Utilisation de kivy
* Importer le module et ses classes
```python
#:kivy 2.0
import kivy
import sys

from kivy.app import App
from kivy.config import Config

from kivy.uix.label import Label
from kivy.uix.widget import Widget
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput


from kivy.uix.boxlayout import BoxLayout
from kivy.uix.gridlayout import GridLayout
from kivy.uix.flexlayout import FlexLayout
from kivy.uix.anchorlayout import AnchorLayout

from kivy.uix.image import Image
from kivy.core.window import Window
from kivy.uix.spinner import Spinner

from kivy.lang import Builder
from kivy.properties import ObjectProperty
```
* Créer fenêtre basique
```python
import kivy
from kivy.app import App
from kivy.uix.label import Label

class MyApp(App):
    def build(self):
        return Label(text="Hello world",font_size=70)
if __name__ == '__main__':
        MyApp().run()
```
* Tableau de 2 colonnes avec text input
```python
import kivy
from kivy.app import App
from kivy.uix.label import Label
from kivy.uix.gridlayout import GridLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button

class MyApp(App):
    def build(self):
        return MyGridLayout()

class MyGridLayout(GridLayout):
    def __init__(self, **kwargs):
         super(MyGridLayout,self).__init__(**kwargs)
         #Colonnes
         self.cols = 2
         #Widgets
         self.add_widget(Label(text="Name: "))
         #Input box
         self.name = TextInput(multiline=False)
         self.add_widget(self.name)
         self.add_widget(Label(text="Pizza: "))
         #Input box
         self.pizza = TextInput(multiline=False)
         self.add_widget(self.pizza)

if __name__ == '__main__':
        MyApp().run()
```
* Deux inputs sur 2 colonnes divisés en 2 (nom et input) et gros bouton submit en bas 
```python
import kivy
from kivy.app import App
from kivy.uix.label import Label
from kivy.uix.gridlayout import GridLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button

class MyApp(App):
    def build(self):
        return MyGridLayout()
        
class MyGridLayout(GridLayout):
    def __init__(self, **kwargs):
         super(MyGridLayout,self).__init__(**kwargs)
         
         #Colonnes
         self.cols = 1

         #Second layout
         self.top_grid = GridLayout()
         self.top_grid.cols = 2

         #Widget nom
         self.top_grid.add_widget(Label(text="Name: "))
         self.name = TextInput(multiline=False)
         self.top_grid.add_widget(self.name)

        #Widget pizza
         self.top_grid.add_widget(Label(text="Pizza: "))
         self.pizza = TextInput(multiline=False)
         self.top_grid.add_widget(self.pizza)

         #Ajout top_grid dans l'application
         self.add_widget(self.top_grid)

        #Button submit

         self.submit = Button(text="Submit",font_size=32)
         self.submit.bind(on_press=self.press)
         self.add_widget(self.submit)

    def press(self,instance):
        name = self.name.text
        pizza = self.pizza.text
        print(f'Hello {name}, you like {pizza}')

        #Ajout text sur l'application
        self.add_widget(Label(text=f'Hello{name}, you like {pizza}'))

        #Clear le champ input après validation
        self.name.text = ""
        self.pizza.text = ""

if __name__ == '__main__':

        MyApp().run()
```
* Ajuster hauteurs et largeur des grid
```python

```
# 2) UIX
* Label = Zone de texte non modifiable
* Text_input = Zone editable
* Bouton = Zone cliquable pour déclencher une action
* On créer widget et on ajout ajoute à l'intérieur de lui d'autres UIX via `.add_widget(uix2)`
## Position et taille des Widgets
* Exemple 
```python
from kivy.app import App 
from kivy.config import Config 
from kivy.uix.widget import Widget 
from kivy.uix.button import Button 

Config.set('graphics', 'width', '300') Config.set('graphics', 'height', '150') 

class WidgetApp(App): 
	def build(self): 
		root = Widget()
		btn1 = Button(text='Btn1',pos=(25,25))
		btn2 = Button(text='Btn2',pos=(135,25)) 
		root.add_widget(btn1)
		root.add_widget(btn2)
		return root

if __name__ == '__main__': 
	WidgetApp().run()
```
## Button
* Propriétés de Button :
	* pos=(x,y) --> Emplacement 
	* size_hint = (.X,.X) --> Taille relative
	* background_color = (r, g, b, a)
	* font_size='XX' --> Taille police
* Exemple 
```python
from kivy.app import App 
from kivy.uix.button import Button  

class KivyButton(App): 
	def build(self): 
		return Button(text='IUT R&T', font_size='35',pos=(100,1000),size_hint=(.30,.12),background_color=(1,0,0,1))

KivyButton().run()
```
## TextInput
* Permet de faire apparaître un champ de texte où l'on peut écrire
* Exemple 
```python
from kivy.app import App 
from kivy.uix.textinput import TextInput  

class TextInputApp(App): 
	def build(self): 
		b = Widget()
		texte = TextInput()
		b.add_widget(texte)
		return b

KivyButton().run()
```

# 3) Dimensions fenêtre et icon
* Gérer taille de fenêtre 
```python
from kivy.config import Config
```
* Redimensionner la fenêtre
```python
Config.set('graphics', 'resizable', '0')
```
* Fixer la largeur de la fenêtre en pixels
```python
Config.set('graphics', 'width', '500')
```
* Fixer la hauteur de la fenêtre en pixels
```python
Config.set('graphics', 'height', '500')
```
* Exemple 
```python
from kivy.config import Config 
from kivy.app import App 
from kivy.uix.button import Label 

Config.set('graphics', 'width', '300') Config.set('graphics', 'height', '150') 

class HelloApp(App): 
	def build(self): 
		self.title = 'Titre!' 
		self.icon = 'logo.png' 
		return Label(text='Bonjour tout le monde!') 

if __name__ == '__main__': 
	HelloApp().run()
```
* Définir titre fenêtre 
```python
self.title = 'Titre!' 
```
* Définir logo de la fenetre
```python
self.icon = 'icon.png!' 
```
# Les conteneurs (grid,flex,box)
* Box Layout
```python
from kivy.app import App 
from kivy.uix.button import Label 
from kivy.uix.boxlayout import BoxLayout 
from kivy.uix.textinput import TextInput 

class LoginApp(App): 
	def build(self): 
		#Titre fenêtre et logo
		self.title = 'Page de connexion!' 
		self.icon = 'connexion.png' 
		#Conteneur en mode horizontal
		box = BoxLayout(orientation="horizontal")
		#Composants graphiques
		box.add_widget(Label(text='Identifiant'))
		box.add_widget(TextInput()
		box.add_widget(Button(text='Valider'))
		return box
		 

if __name__ == '__main__': 
	LoginApp().run()
```
* Grid Layout
```python
from kivy.app import App 
from kivy.uix.button import Label 

class LoginApp(App): 
	def build(self): 
		#Titre fenêtre et logo
		self.title = 'Page de connexion!' 
		self.icon = 'connexion.png' 
		#Conteneur en mode horizontal
		box = BoxLayout(orientation="horizontal")
		#Composants graphiques
		box.add_widget(Label(text='Identifiant'))
		box.add_widget(TextInput()
		box.add_widget(Button(text='Valider'))
		return box
		 

if __name__ == '__main__': 
	LoginApp().run()
```

# Images
* Propriétés image kivy
	* source
	* size_hint
	* pos_hint
* Exemple 
```python
from kivy.app import App 
from kivy.uix.image import Image

class MainApp(App): 
	def build(self): 
		img = Image(source='logo.png',size_hint(.1,.5),pos_hint={'center_x':.5,'center_y':.5})
		return img
		
if __name__ == '__main__': 
	app = MainApp()
	app.run()
```

# Centrer un élément :
```python
pos_hint={'center_x':0.5, 'center_y':0.5}
color: red,green,blue,opacity --> 0-1
background_color: (0/255, 255/255, 255/255, 1) --> Uniquement sur TextInput et Button
font_size: 45
```
* Modifier backgroundcolor de la fenetre
```
Window.clearcolor = (255/255.0, 255/255.0, 255/255.0)
```
* Modifier background color d'un label :
```
Label:
	text: "Votre taille:"
	color: "#2BE5CE"
    canvas.before:
    Color:
		rgba: 0, 1, 1, 1 
	Rectangle:
		pos: self.pos
		size: self.size
```
* Créer button avec angle 
```
<RoundedButton@Button>
    background_color: (0,0,0,0)
    background_normal: ''
    canvas.before:
        Color:
            rgba: (0/255,255/255,255/255,1)
        RoundedRectangle:
            size: self.size
            pos: self.pos
            radius: [58]
```