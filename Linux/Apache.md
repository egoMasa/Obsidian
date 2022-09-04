# Guide Apache Linux

## I) Principe et explication

* Apache HTTP Server (Apache) est un ***serveur HTTP***, probablement le plus connu au monde. Le nom complet est bien "Apache HTTP Server", mais souvent par abus de langage, on le nomme en raccourci "Apache". Cela peut parfois porter à confusion avec d'autres logiciels de la fondation éponyme, qui gère le développement de ce serveur web mais aussi, entre autres, d'Open Office (suite bureautique), Tomcat (serveur d'applications) et de nombreux logiciels utilisés en environnement Java (comme log4j).

## II) Installation

* Vérifier services réseaux fonctionnant
```bash
(machine)root#lsof -n -i
```
* Installer paquet ***apache2***
```bash
(machine)root#apt-get install apache2
```
* Démarrer service apache2
```bash
(machine)root#sytemctl start apache2
```
* Arréter service apache2
```bash
(machine)root#sytemctl stop apache2
```
* Activer au démarrage service apache2
```bash
(machine)root#sytemctl enable apache2
```
* Accéder à la page web basique apache
```navigateur
http://@ip_machine
```


## II) Fichiers de configuration d'apache2

-   `sites-available` contient les fichiers de configuration des **sites** disponibles
    ```bash
<VirtualHost *:80>

	ServerName example.com
	
	ServerAlias www.example.com
	DocumentRoot "/var/www/example"
	
	ErrorLog /var/log/apache2/error.example.com.log
	CustomLog /var/log/apache2/access.example.com.log combined
	
</VirtualHost>
```
-   `sites-enabled` contient des [liens symboliques](https://doc.ubuntu-fr.org/lien_physique_et_symbolique "lien_physique_et_symbolique") vers les configurations, dans `site-available`, des sites activés
    

-   `conf-available` contient les fichiers de configuration des **autres services** disponibles
    
-   `conf-enabled` contient des [liens symboliques](https://doc.ubuntu-fr.org/lien_physique_et_symbolique "lien_physique_et_symbolique") vers les configurations, dans `conf-available`, des autres services activés
    

-   `mods-available` contient les fichiers de configuration des **modules** d'Apache disponibles
    
-   `mods-enabled` contient des [liens symboliques](https://doc.ubuntu-fr.org/lien_physique_et_symbolique "lien_physique_et_symbolique") vers les configurations, dans `mods-available`, des modules activés

* ``var/www/example/`` = Répertoire des fichiers HTML/CSS/JS...

## III) Activation et désactivation sites/services/mods

* `a2ensite [nom_fichier]` = Enable un site available
* `a2dissite [nom_fichier]` = Disable un site enable
* Lister les sites
```bash
cat /etc/apache2/mods-available
```

* `a2enconf [nom_configuration]` = Enable une configuration
* `a2disconf [nom_configuration]` = Disable une configuration
* Lister les services
```bash
cat /etc/apache2/conf-available
```

* `a2enmod [nom_module]` = Enable un module
* `a2dismod [nom_module]` = Disable un module 
* Lister les sites
```bash
cat /etc/apache2/mods-available
```