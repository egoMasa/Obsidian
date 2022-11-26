# Guide MariaDB Linux

* Mettre à jour programme 
```
sudo apt update
```
* Installer MariaDB
```
sudo apt-get install mariadb-server
```
* Vérifier statut du serveur 
```
systemctl status mariadb
```
* Modifier mot de passe root du serveur 
```
mysqladmin -u root password 'NEW_PASSWD'

ou 

sudo mysql
# Selection BD "mysql" où sont les users
use mysql
# Requête de modification
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('NEW_PASSWD');
# Prendre en compte les modifications
FLUSH PRIVILEGES;
quit;
mysql -u root -p
```

# Création BDD 
* Création de la base de données
```SQL
CREATE DATABASE base_name
```
* Activer base de données
```SQL
USE base_name
```
* Créer table
```SQL
CREATE TABLE "nom_table"
("colonne1" "type_donnees",
 "colonne2" "type_donnees"
)
```
* Insérer données dans table
```SQL
INSERT INTO "nom_table" ("colonne1","colonne2") VALUES ("valeur1","valeur2")
```
* Exemple de BDD
```SQL
CREATE DATABASE hotel;
USE hotel;

CREATE TABLE clients 
(num_client int not null auto_increment, 
 nom varchar(25), 
 prenom varchar(20), 
 telephone bigint, 
 PRIMARY KEY (num_client));

INSERT INTO clients (nom, prenom, telephone) VALUES ("Jean", "Paul", 12345678), 
("Gaston", "Yves", 23456789), 
("Zoro", "Zoro", 13579860), 
("Paul", "Jean", 987654321), 
("Jerome", "Jean", 87654321);
```
# Afficher données
* Syntaxe globale
```SQL
SELECT {ALL,*,nom_col,DISTINCT nom_col} 
FROM {table}
GROUP BY ...
HAVING ...
ORDER BY
```
# Type de données
* INT : Nombre entier 
* FLOAT,DOUBLE : Nombre à virgule
* CHAR : Chaine de caractères
* DATE : date et heure
* ENUM : Enumération à partir d'une liste
* SET : Ensemble à partir d'une liste
* BLOB : Stocker informations binaires (img)
* LONGTEXT
* VARCHAR(X)