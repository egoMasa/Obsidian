# Guide format XML python
## Syntaxe générale 
![[Pasted image 20220828125345.png]]
  

## Module de traitement : ***xml.etree.ElementTree***
* Importer module
<code>import xml.etree.ElementTree  as ET</code>

## Méthodes
* Charger fichier XML
<code>document = ET.parse("fichier.xml")</code>

* Obtenir racine du fichier
<code>racine = document.getroot()</code>

* Affichier nom balise
<code>racine.tag</code>

* Afficher attribut dans balise
<code>racine.attrib</code>

* Afficher texte dans balise
<code>racine.text</code>

* Seletionner toutes les balises avec nom precis
<code>racine.iter("nom_balise")</code>
<code>racine.findall("nom_balise")</code>

* Ajouter une balise enfant
<code>nouveau = ET.SubElement(parent,"nom",{"attribut":"valeur"}</code>

* Ajouter texte à une balise 
<code>balise.text = "valeur"</code>

* Sauvegarder modifications
<code>document.write("fichier.xml",encoding="utf-8"</code>