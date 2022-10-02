# Guide SQL

## I)Sommaire
### I) Listes Commandes 

1.  SELECT (DISTINCT, NO_CACHE)
2.  WHERE (AND&OR, IN, BETWEEN, LIKE, IS NULL, IS NOT NULL, %)
3.  GROUP BY(WITHROLLUP)
4.  HAVING
5.  ORDER BY
6.  AS (alias)
7.  LIMIT
8.  CASE
9.  UNION (UNION ALL)
10.  INTERSECT
11.  EXCEPT/MINUS
12.  INSERT INTO ( ON DUPLICATE KEY UPDATE)
13.  UPDATE
14.  DELETE
15.  MERGE
16.  TRUNCATE
17.  CREATE DATABASE
18.  DROP DATABASE
19.  CREATE TABLE (PRIMARY KEY, AUTO_INCREMENT)
20.  ALTER TABLE
21.  DROP TABLE
22.  Jointure SQL ({INNER,CROSS,LEFT,RIGHT,FULL,SELF,NATURAL} JOIN)
23.  SQL Sous-requête ( EXISTS, ALL, ANY/SOME)
24.  Index SQL (CREATE INDEX)
25.  EXPLAIN
26.  OPTIMIZE

### II) Fonctions SQL

1.  Fonctions d’agrégation (AVG, COUNT, MAX, MIN, SUM)
2.  Fonctions de chaînes de caractères (CONCAT, LENGTH, REPLACE, SOUNDEX, SUBSTRING, LEFT, RIGHT, REVERSE, TRIM, LTRIM, RTRIM, LPAD, UPPER, LOWER, UCASE, LCASE, LOCATE, INSTR)

---
## II)Commandes et fonctions
1)***SELECT***: Retourne un ou plusieurs éléments dans une ou plusieurs colonnes d’une table.

* Syntaxe générale simple: 
```SQL
SELECT nom_du_champ FROM nom_du_tableau
```
* Syntaxe générale complète
```SQL
SELECT *
FROM table
WHERE condition
GROUP BY expression
HAVING condition
{ UNION | INTERSECT | EXCEPT }
ORDER BY expression
LIMIT count
OFFSET start
```
* Exemple 
```SQL
SELECT ville FROM client
```
* Obtenir plusieurs colonnes:
```SQL
SELECT prenom, nom FROM client
```
* Obtenir toutes les colonnes d’un tableau:
```SQL
SELECT * FROM client
```
* Eviter d’afficher les lignes en double: 
```SQL
SELECT DISTINCT ma_colonne
FROM nom_du_tableau
```

---
2)***WHERE***: Extraire les lignes selon une condition
* Syntaxe générale
```SQL
SELECT nom_colonnes FROM nom_table WHERE condition
```
* Exemple
```SQL
SELECT * FROM client WHERE ville = 'paris'
```
* Opérateurs de comparaisons:
![](https://lh3.googleusercontent.com/Prk42ESctEcVyXawuEk3xkg6Sxzj7s9txUFZZ6ag4Wj8n4609B36E-zwcT7m_-ubauxIcUXjiveN4i172a-8Z7KUHfl49GSt3IP1DwaBkp69n66gkgx0_EBaK-Ot-Oah_oZVTwG2RG7abQ5Bry2lBZCkIjv5ZhIFnwlFnXeJ5ZULfjMq0-qnkm9V)

* Combiner des conditions avec ***AND***
```SQL
SELECT nom_colonnes
FROM nom_table
WHERE condition1 AND condition2
```
* Combiner des conditions avec ***OR***
```SQL
SELECT nom_colonnes FROM nom_table
WHERE condition1 OR condition2
```
* Combiner des conditions avec ***AND*** et ***OR***
```SQL
SELECT nom_colonnes FROM nom_table
WHERE condition1 AND (condition2 OR condition3)
```
* Tester l’appartenance avec ***WHERE IN***
```SQL
SELECT nom_colonne
FROM table
WHERE nom_colonne IN ( valeur1, valeur2, valeur3, ... )
```
* Exemple
```SQL
SELECT *
FROM adresse
WHERE addr_ville IN ( 'Paris', 'Graimbouville' )
```
* Sélectionner une intervalle avec ***WHERE BETWEEN***: 
```SQL
SELECT *
FROM table
WHERE nom_colonne BETWEEN 'valeur1' AND 'valeur2'
```
* Recherche avec modèle (qui commence par, termine par, etc…) avec ***WHERE LIKE***

```SQL
SELECT *
FROM table
WHERE colonne LIKE modèle
```

* Précision sur ***LIKE***
	-   ***LIKE ‘%a’*** : le caractère “%” est un caractère joker qui remplace tous les autres caractères. Ainsi, ce modèle permet de rechercher toutes les chaines de caractère qui se termine par un “a”.
	    
	-   ***LIKE ‘a%’*** : ce modèle permet de rechercher toutes les lignes de “colonne” qui commence par un “a”.
	    
	-   ***LIKE ‘%a%’*** : ce modèle est utilisé pour rechercher tous les enregistrement qui utilisent le caractère “a”.
	    
	-   ***LIKE ‘pa%on’*** : ce modèle permet de rechercher les chaines qui commence par “pa” et qui se terminent par “on”, comme “pantalon” ou “pardon”.
	    
	-   ***LIKE ‘a_c’*** : peu utilisé, le caractère “_” (underscore) peut être remplacé par n’importe quel caractère, mais un seul caractère uniquement (alors que le symbole pourcentage “%” peut être remplacé par un nombre incalculable de caractères . Ainsi, ce modèle permet de retourner les lignes “aac”, “abc” ou même “azc”.
	    

* Filtrer les résultats avec valeur NULL avec ***WHERE IS NULL / IS NOT NULL***
```SQL
SELECT *
FROM `table
WHERE nom_colonne IS NULL
```
---

3)***GROUP BY***: Grouper plusieurs résultats et utiliser des fonctions sur le groupement

* Syntaxe générale:
```SQL
SELECT colonne1, fonction(colonne2)
FROM table
GROUP BY colonne1
```
* Exemple
```SQL
SELECT client, SUM(tarif)
FROM achat
GROUP BY client
```

* Fonctions de statistiques:

	-   ***AVG()*** pour calculer la moyenne d’un set de valeur. Permet de connaître le prix du panier moyen pour de chaque client
 
	-   ***COUNT()*** pour compter le nombre de lignes concernées. Permet de savoir combien d’achats a été effectué par chaque client
	    
	-   ***MAX()*** pour récupérer la plus haute valeur. Pratique pour savoir l’achat le plus cher
	    
	-   ***MIN()*** pour récupérer la plus petite valeur. Utile par exemple pour connaître la date du premier achat d’un client
	    
	-   ***SUM()*** pour calculer la somme de plusieurs lignes. Permet par exemple de connaître le total de tous les achats d’un client

* Ajouter une ligne suplémentaire “super-agrégateur” avec ***GROUP BY WITH ROLLUP***
```SQL
SELECT colonne1, fonction(colonne2)
FROM table
GROUP BY colonne1 WITH ROLLUP
```

Exemple:
```SQL
SELECT `code_postal`, COUNT(*), SUM(`commande`), MIN(`date_inscription`), MAX(`date_inscription`)
FROM `client`
GROUP BY code_postal WITH ROLLUP
```

---
4)***HAVING***: Comme WHERE mais filtre en utilisant fonctions (SUM, COUNT, AVG, MIN, MAX)

* Syntaxe générale:
```SQL
SELECT colonne1, SUM(colonne2)
FROM nom_table
GROUP BY colonne1
HAVING fonction(colonne2) operateur valeur
```

* Exemple:
```SQL
SELECT client, SUM(tarif)
FROM achat
GROUP BY client
HAVING SUM(tarif) > 40
```
---
5)***ORDER BY***: trier les lignes dans un résultat d’une requête

* Syntaxe générale : Ascendant 
```SQL
SELECT colonne1, colonne2
FROM table
ORDER BY colonne1 ASC
```
* Syntaxe générale : Descendant 
```SQL
SELECT colonne1, colonne2
FROM table
ORDER BY colonne1 DESC
```
* Exemple:
```SQL
SELECT *
FROM utilisateur
ORDER BY nom
```
---
6)***AS***: Renommer une colonne ou table, sert à faciliter

* Syntaxe pour colonne :

```SQL
SELECT colonne1 AS c1, colonne2
FROM `table`
```

* Syntaxe pour table:
```SQL
SELECT * 
FROM `nom_table` AS t1
```

---
7)***LIMIT***: Spécifier nombre maximum de résultat d’une requête, on peut y mettre un offset(décalage)

* Syntaxe générale:

```SQL
SELECT *
FROM table
LIMIT 10
```
---

9)***UNION***: Mettre bout à bout les résultats de plusieurs requêtes 

* Syntaxe générale 

```SQL
SELECT * FROM table1 UNION SELECT * FROM table2
```
---

9)***INTERSECT***: Obtenir l’intersection des résultats de 2 requêtes, pour voir les données similaires

* Syntaxe générale:
```SQL
SELECT * FROM `table1`
INTERSECT
SELECT * FROM `table2`
```
* Exemple:
```SQL
SELECT * FROM `magasin1_client`
INTERSECT
SELECT * FROM `magasin2_client`
```
---

11)INSERT INTO: Pour L’insertion de données dans une table, permet d’inclure une seule ligne ou plusieurs

Syntaxe une ligne en spécifiant toutes les colonnes :

#INSERT INTO table VALUES ('valeur 1', 'valeur 2', ...)

Syntaxe une ligne en spécifiant seulement les colonnes souhaitées :

INSERT INTO table (nom_colonne_1, nom_colonne_2, ...

 VALUES ('valeur 1', 'valeur 2', ...)

Syntaxe plusieurs lignes:

INSERT INTO client (prenom, nom, ville, age)

 VALUES

 ('Rébecca', 'Armand', 'Saint-Didier-des-Bois', 24),

 ('Aimée', 'Hebert', 'Marigny-le-Châtel', 36),

 ('Marielle', 'Ribeiro', 'Maillères', 27),

 ('Hilaire', 'Savary', 'Conie-Molitard', 58);

---

12)UPDATE: Effectuer des modifications sur des lignes existantes souvent utilisés avec WHERE

Syntaxe:

UPDATE table

SET nom_colonne_1 = 'nouvelle valeur'

WHERE condition

Exemple:

UPDATE client

SET rue = '49 Rue Ameline',

  ville = 'Saint-Eustache-la-Forêt',

  code_postal = '76210'

WHERE id = 2

  

---

  

13)DELETE: Permet de supprimer des lignes dans une table

Syntaxe:

DELETE FROM `table`

WHERE condition

Exemple supprimer une ligne:

DELETE FROM `utilisateur`

WHERE `id` = 1

  

Exemple supprimer plusieurs lignes:

DELETE FROM `utilisateur`

WHERE `date_inscription` < '2012-04-10'

Exemple supprimer toutes les données:

DELETE FROM `utilisateur`

  

---

  
  

14)MERGE: Insérer ou mettre à jour des données d’une table

Syntaxe:

MERGE INTO table1

  USING table_reference

  ON (conditions)

  WHEN MATCHED THEN

    UPDATE SET table1.colonne1 = valeur1, table1.colonne2 = valeur2

    DELETE WHERE conditions2

  WHEN NOT MATCHED THEN

    INSERT (colonnes1, colonne3) 

    VALUES (valeur1, valeur3)

Explications:

1.  MERGE INTO permet de sélectionner la table à modifier
    
2.  USING et ON permet de lister les données sources et la condition de correspondance
    
3.  WHEN MATCHED permet de définir la condition de mise à jour lorsque la condition est vérifiée
    
4.  WHEN NOT MATCHED permet de définir la condition d’insertion lorsque la condition n’est pas vérifiée
    

  
  

---

  

15)TRUNCATE TABLE: Permet de supprimer toutes les données d’une table sans supprimer la table elle-même

Syntaxe:

TRUNCATE TABLE `table`

Exemple:

TRUNCATE TABLE `fourniture`

  
  
  
  

---

  

16)CREATE DATABASE: Pour créer une base de données 

Syntaxe:

CREATE DATABASE ma_base

---

  

17)DROP DATABASE: Pour supprimer totalement une SBGD

Syntaxe:

DROP DATABASE ma_base

  

---

  

18)CREATE TABLE: Créer une table 

Syntaxe:

CREATE TABLE nom_de_la_table

(

    colonne1 type_donnees,

    colonne2 type_donnees,

    colonne3 type_donnees,

    colonne4 type_donnees

)

Exemple:

CREATE TABLE utilisateur

(

    id INT PRIMARY KEY NOT NULL,

    nom VARCHAR(100),

    prenom VARCHAR(100),

    email VARCHAR(255),

    date_naissance DATE,

    pays VARCHAR(255),

    ville VARCHAR(255),

    code_postal VARCHAR(5),

    nombre_achat INT

)

-PRIMARY KEY: Permet d’identifier chaque enregistrement dans une table, elle doit être unique et doit pas contenir de valeur NULL

Syntaxe:

CREATE TABLE `nom_de_la_table` (

  id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,

  [...]

);

  

---

  
  

19)ALTER TABLE: Modifier une table existante pour ajouter une colonne, supprimer une colonne ou la modifier 

-Syntaxe:

ALTER TABLE nom_table

instruction

-Ajouter une colonne:

ALTER TABLE nom_table

ADD nom_colonne type_donnees

Exemple ajouter une colonne:

ALTER TABLE utilisateur

ADD adresse_rue VARCHAR(255)

-Supprimer une colonne:

ALTER TABLE nom_table

DROP nom_colonne

-Modifier une colonne:

ALTER TABLE nom_table

MODIFY nom_colonne type_donnees

Renommer une colonne:

ALTER TABLE nom_table

CHANGE colonne_ancien_nom colonne_nouveau_nom type_donnees

  
  

---

  

20) DROP TABLE: Supprimer définitivement une table

Syntaxe:

DROP TABLE nom_table

  

---

  
  
  

22)Jointure SQL: Pour associer plusieurs tables dans une même requête, combiner des résultats

Types de jointures:

-   [INNER JOIN](https://sql.sh/cours/jointures/inner-join) : jointure interne pour retourner les enregistrements quand la condition est vrai dans les 2 tables. C’est l’une des jointures les plus communes.
    

SELECT *

FROM table1

INNER JOIN table2 ON table1.id = table2.fk_id

-   [CROSS JOIN](https://sql.sh/cours/jointures/cross-join) : jointure croisée permettant de faire le produit cartésien de 2 tables. En d’autres mots, permet de joindre chaque lignes d’une table avec chaque lignes d’une seconde table. Attention, le nombre de résultats est en général très élevé.
    

SELECT *

FROM table1

CROSS JOIN table2

-   [LEFT JOIN](https://sql.sh/cours/jointures/left-join) (ou LEFT OUTER JOIN) : jointure externe pour retourner tous les enregistrements de la table de gauche (LEFT = gauche) même si la condition n’est pas vérifié dans l’autre table.
    

SELECT *

FROM table1

LEFT JOIN table2 ON table1.id = table2.fk_id

-   [RIGHT JOIN](https://sql.sh/cours/jointures/right-join) (ou RIGHT OUTER JOIN) : jointure externe pour retourner tous les enregistrements de la table de droite (RIGHT = droite) même si la condition n’est pas vérifié dans l’autre table.
    

SELECT *

FROM table1

RIGHT JOIN table2 ON table1.id = table2.fk_id

-   [FULL JOIN](https://sql.sh/cours/jointures/full-join) (ou FULL OUTER JOIN) : jointure externe pour retourner les résultats quand la condition est vrai dans au moins une des 2 tables.
    

SELECT *

FROM table1

FULL JOIN table2 ON table1.id = table2.fk_id

-   [SELF JOIN](https://sql.sh/cours/jointures/self-join) : permet d’effectuer une jointure d’une table avec elle-même comme si c’était une autre table.
    

SELECT `t1`.`nom_colonne1`, `t1`.`nom_colonne2`, `t2`.`nom_colonne1`, `t2`.`nom_colonne2`

FROM `table` as `t1`

LEFT OUTER JOIN `table` as `t2` ON `t2`.`fk_id` = `t1`.`id`

-   [NATURAL JOIN](https://sql.sh/cours/jointures/natural-join) : jointure naturelle entre 2 tables s’il y a au moins une colonne qui porte le même nom entre les 2 tables SQL
    

SELECT *

FROM table1

NATURAL JOIN table2

-   UNION JOIN : jointure d’union
    

  
  

---

27)SQL Sous-requête ( EXISTS, ALL, ANY/SOME): consiste à exécuter une requête à l’intérieur d’une autre requête. Une requête imbriquée est souvent utilisée au sein d’une clause WHERE ou de HAVING pour remplacer une ou plusieurs constantes.

Syntaxe requête imbriquée qui retourne un seul résultat:

SELECT *

FROM `table`

WHERE `nom_colonne` = (

    SELECT `valeur`

    FROM `table2`

    LIMIT 1

  )

Syntaxe requête imbriquée qui retourne une colonne:

SELECT *

FROM `table`

WHERE `nom_colonne` IN (

    SELECT `colonne`

    FROM `table2`

    WHERE `cle_etrangere` = 36

  )

-EXISTS: s’utilise dans une clause conditionnelle pour savoir s’il y a une présence ou non de lignes lors de l’utilisation d’une sous-requête.

Syntaxe:

SELECT nom_colonne1

FROM `table1`

WHERE EXISTS (

    SELECT nom_colonne2

    FROM `table2`

    WHERE nom_colonne3 = 10

  )

Exemple:

SELECT *

FROM commande

WHERE EXISTS (

    SELECT * 

    FROM produit 

    WHERE c_produit_id = p_id

)

  
  
  
  

-ALL: Permet de comparer une valeur dans l’ensemble de valeurs d’une sous-requête. En d’autres mots, cette commande permet de s’assurer qu’une condition est “égale”, “différente”, “supérieure”, “inférieure”, “supérieure ou égale” ou “inférieure ou égale” pour tous les résultats retourné par une sous-requête.

Syntaxe:

SELECT *

FROM table1

WHERE condition > ALL (

    SELECT *

    FROM table2

    WHERE condition2

)

Exemple:

SELECT colonne1

FROM table1

WHERE colonne1 > ALL (

    SELECT colonne1

    FROM table2

)

-ANY/SOME: Permet de comparer une valeur avec le résultat d’une sous-requête. Il est ainsi possible de vérifier si une valeur est “égale”, “différente”, “supérieur”, “supérieur ou égale”, “inférieur” ou “inférieur ou égale” pour au moins une des valeurs de la sous-requête.

Syntaxe:

SELECT *

FROM table1

WHERE condition > ANY (

    SELECT *

    FROM table2

    WHERE condition2

)

Exemple:

SELECT colonne1

FROM table1

WHERE colonne1 > ANY (

    SELECT colonne1

    FROM table2

)

  

---

  
  
  
  
  
  

28)CREATE INDEX: 

Syntaxe index ordinaire:

CREATE INDEX `index_nom` ON `table`;

Syntaxe index unique:

CREATE UNIQUE INDEX `index_nom` ON `table` (`colonne1`);

Convention de nommage:

-   Préfixe “PK_” pour Primary Key (traduction : clé primaire)
    
-   Préfixe “FK_” pour Foreign Key (traduction : clé étrangère)
    
-   Préfixe “UK_” pour Unique Key (traduction : clé unique)
    
-   Préfixe “UX_” pour Unique Index (traduction : index unique)
    
-   Préfixe “IX_” pour chaque autre IndeX
    

  
  

---

29)EXPLAIN: Permet d’afficher le plan d’exécution d’une requête SQL. Cela permet de savoir de quelle manière le Système de Gestion de Base de Données (SGBD) va exécuter la requête et s’il va utiliser des index et lesquels.

Syntaxe:

EXPLAIN SELECT *

FROM `user`

ORDER BY `id` DESC

Exemple:

CREATE TABLE IF NOT EXISTS `timezones` (

 `timezone_id` int(10) unsigned NOT NULL AUTO_INCREMENT,

 `timezone_groupe_fr` varchar(50) DEFAULT NULL,

 `timezone_groupe_en` varchar(50) DEFAULT NULL,

 `timezone_detail` varchar(100) DEFAULT NULL,

 PRIMARY KEY (`timezone_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=698;

Analyse de la requête SQL:

EXPLAIN SELECT timezone_groupe_fr, COUNT(timezone_detail) AS total_timezone

FROM `timezones` 

GROUP BY timezone_groupe_fr

ORDER BY timezone_groupe_fr ASC

Explications:

-   id : identifiant de SELECT
    
-   select_type : type de cause SELECT (exemple : SIMPLE, PRIMARY, UNION, DEPENDENT UNION, SUBQUERY, DEPENDENT SUBSELECT ou DERIVED)
    
-   table : table à laquelle la ligne fait référence
    
-   type : le type de jointure utilisé (exemple : system, const, eq_ref, ref, ref_or_null, index_merge, unique_subquery, index_subquery, range, index ou ALL)
    
-   possible_keys : liste des index que MySQL pourrait utiliser pour accélérer l’exécution de la requête. Dans notre exemple, aucun index n’est disponible pour accélérer l’exécution de la requête SQL
    
-   key : cette colonne présente les index que MySQL a décidé d’utiliser pour l’exécution de la requête
    
-   key_len : indique la taille de la clé qui sera utilisée. S’il n’y a pas de clé, cette colonne renvois NULL
    
-   ref : indique quel colonne (ou constante) sont utilisés avec les lignes de la table
    
-   rows : estimation du nombre de ligne que MySQL va devoir analyser examiner pour exécuter la requête
    
-   Extra : information additionnelle sur la façon dont MySQL va résoudre la requête. Si cette colonne retourne des résultats, c’est qu’il y a potentiellement des index à utiliser pour optimiser les performances de la requête SQL. Le message “using temporary” permet de savoir que MySQL va devoir créer une table temporaire pour exécuter la requête. Le message “using filesort” indique quant à lui que MySQL va devoir faire un autre passage pour retourner les lignes dans le bon ordre
    

  

Ajout d’un index:

ALTER TABLE `timezones` ADD INDEX ( `timezone_groupe_fr` );

  

---

  

30)OPTIMIZE: Permet de réorganiser le stockage physique des données et reconstruit les index.

Syntaxe:

OPTIMIZE TABLE nom_table;

Exemple:

DELETE FROM location_log

WHERE id < 5000;

OPTIMIZE TABLE location_log;

  

---

  
  
  
  
  
  
  
  

III)Fonctions SQL

1)Fonctions d’agrégation (AVG, COUNT, MAX, MIN, SUM):  permettent d’effectuer des opérations statistiques sur un ensemble d'enregistrements. Étant données que ces fonctions s’appliquent à plusieurs lignes en même temps, elles permettent des opérations qui servent à récupérer l’enregistrement le plus petit, le plus grand ou bien encore de déterminer la valeur moyenne sur plusieurs enregistrement.

  

Liste des fonctions d’agrégation statistiques:

-   [AVG()](https://sql.sh/fonctions/agregation/avg) pour calculer la moyenne sur un ensemble d’enregistrement
    

SELECT AVG(nom_colonne) FROM nom_table

Exemple:

SELECT client, AVG(tarif)

 FROM achat

 GROUP BY client

  

-   [COUNT()](https://sql.sh/fonctions/agregation/count) pour compter le nombre d’enregistrement sur une table ou une colonne distincte
    

SELECT COUNT(nom_colonne) FROM table

Exemple:SELECT COUNT(*) FROM utilisateur

  
  

-   [MAX()](https://sql.sh/fonctions/agregation/max) pour récupérer la valeur maximum d’une colonne sur un ensemble de ligne. Cela s’applique à la fois pour des données numériques ou alphanumérique
    

SELECT MAX(nom_colonne) FROM table

Exemple: SELECT MAX(prix) FROM produit

  

-   [MIN()](https://sql.sh/fonctions/agregation/min) pour récupérer la valeur minimum de la même manière que MAX()
    

SELECT MIN(nom_colonne) FROM table

Exemple:

SELECT MIN(prix)

FROM `produits`

WHERE `categorie` = 'maison'

  

-   [SUM()](https://sql.sh/fonctions/agregation/sum) pour calculer la somme sur un ensemble d’enregistrement
    

SELECT SUM(nom_colonne)

FROM table

Exemple:

SELECT SUM(prix) AS prix_total

FROM facture

WHERE facture_id = 1

  

Syntaxe:

SELECT fonction(colonne) FROM table

---

  

Fonctions de chaînes de caractères (CONCAT, LENGTH, REPLACE, SOUNDEX, SUBSTRING, LEFT, RIGHT, REVERSE, TRIM, LTRIM, RTRIM, LPAD, UPPER, LOWER, UCASE, LCASE, LOCATE, INSTR,...)

-   [CONCAT()](https://sql.sh/fonctions/concat) concaténer plusieurs chaînes de caractères [MySQL, PostgreSQL, SQL Server]
    

  

Avec SELECT:

SELECT id, CONCAT(colonne1, colonne2)

FROM `table`

Avec WHERE:

SELECT id

FROM `table`

WHERE colonne1 = CONCAT(colonne2, colonne3)

  

-   [INSTR()](https://sql.sh/fonctions/instr) retourne la position d’une occurrence dans une chaîne de caractères [MySQL]
    
-   [LCASE()](https://sql.sh/fonctions/lcase) synonyme de LOWER() [MySQL]
    
-   [LEFT()](https://sql.sh/fonctions/left) retourner les n premiers caractères d’une chaîne de caractères [MySQL, PostgreSQL, SQL Server]
    
-   [LENGTH()](https://sql.sh/fonctions/length) retourner la longueur d’une chaîne [MySQL, PostgreSQL]
    

SELECT LENGTH('exemple');

SELECT id, login, ville, telephone

FROM utilisateur

WHERE LENGTH(login) < 5

  

-   [LOCATE()](https://sql.sh/fonctions/locate) retourne la position de la première occurrence de la sous-chaîne [MySQL]
    
-   [LOWER()](https://sql.sh/fonctions/lower) transformer la chaîne pour tout retourner en minuscule [MySQL, PostgreSQL, SQL Server]
    
-   [LPAD()](https://sql.sh/fonctions/lpad) ajouter un contenu spécifié au début d’une chaîne, jusqu’à atteindre la longueur désirée [MySQL, PostgreSQL]
    
-   [LTRIM()](https://sql.sh/fonctions/ltrim) supprimer les caractères vides au début de la chaîne [MySQL, PostgreSQL, SQL Server]
    
-   [REPLACE()](https://sql.sh/fonctions/replace) remplacer des caractères par d’autres caractères [MySQL, PostgreSQL, SQL Server]
    
-   [REVERSE()](https://sql.sh/fonctions/reverse) inverser les caractères d’un chaîne [MySQL, PostgreSQL, SQL Server]
    
-   [RIGHT()](https://sql.sh/fonctions/right) retourner les n derniers caractères d’une chaîne de caractères [MySQL, PostgreSQL, SQL Server]
    
-   [RTRIM()](https://sql.sh/fonctions/rtrim) supprimer les caractères vides en fin d’une chaîne de caractère [MySQL, SQL Server]
    
-   [SOUNDEX()](https://sql.sh/fonctions/soundex) retourner la version SOUNDEX de la chaîne [MySQL, SQL Server]
    
-   [SUBSTR()](https://sql.sh/fonctions/substring) retourne un segment de chaîne [MySQL, PostgreSQL]
    
-   [SUBSTRING()](https://sql.sh/fonctions/substring) retourne un segment de chaîne [MySQL, PostgreSQL, SQL Server]
    
-   [TRIM()](https://sql.sh/fonctions/trim) supprime les caractères vides en début et fin de chaîne [MySQL, PostgreSQL]
    
-   [UCASE()](https://sql.sh/fonctions/ucase) synonyme de UPPER() [MySQL]
    
-   [UPPER()](https://sql.sh/fonctions/upper) tout retourner en majuscule [MySQL, PostgreSQL, SQL Server]

## III) Création base de données
* Créer une base de données
```SQL
CREATE DATABASE IF NOT EXISTS ma_base
```
* Supprimer complétement 
```SQL
DROP DATABASE ma_base
```
* Créer une table de données
```SQL
CREATE TABLE nom_de_la_table
(
    colonne1 type_donnees,
    colonne2 type_donnees,
    colonne3 type_donnees,
    colonne4 type_donnees
)
```
* Exemple
```SQL
CREATE TABLE utilisateur
(
    id INT PRIMARY KEY NOT NULL,
    nom VARCHAR(100),
    prenom VARCHAR(100),
    email VARCHAR(255),
    date_naissance DATE,
    pays VARCHAR(255),
    ville VARCHAR(255),
    code_postal VARCHAR(5),
    nombre_achat INT
)
```
* Modifier table existante (ajout, suppresion, rename)
```SQL
ALTER TABLE nom_table
instruction
```
## IV) Injecter des données
## V) Requête de données