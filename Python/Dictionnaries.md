# Guide pour les dictionnaires python
## Syntaxe générale 
<code>dico  = {"couleur":"rouge","marque":"renault"}</code>
## Méthodes 
* Récuperer cléfs d'un dictionnaire
<code>dico.keys()</code>

* Récuperer valeurs d'un dictionnaire
<code>dico.values()</code>

* Récuperer valeur à partir de la cléf 
<code>dico.get("clef")</code> ou <code>dico["clef"]</code>

* Récuperer cléfs d'un dictionnaire
<code>dico.keys()</code>

* Séparer cléfs et valeurs sous forme de tuple 
<code>dico.items()</code>

* Fusionner deux dictionnaires 
<code>dico.update(dico2)</code>

* Copier un dictionnaire
<code>dico2.copy(dico)</code>

* Ajouter la meme valeur à chaque cléfs d'un dictionnaire
<code>dico2=dict.fromkeys(dico,"valeur")</code>

* Ajouter cléf+valeur
<code>dico["clef"]= valeur</code>