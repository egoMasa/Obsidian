
* Variables d'environnements :
	* $HOME
	* $PWD
	* $SHELL
	* $USER
	* $HOSTNAME
	* $PATH
* Annuler effet carac spécial
`\`
* Guillemet simple : Rien n'est interprété
* Guillemet double : Rien n'est interprété sauf $ 
```bash
echo "repertoire courant 'pwd'"
>> repertoire courant /home/...

echo "repertoire courant $(pwd)"
>> repertoire courant /home/...

echo "valeur de home $HOME"
>> valeur de home /root
```

# PIPE |
* Permet de passer en argumenent d'une commande le resultat d'une autre (précédente)
```shell
ls | tee script.txt
ps -aux | grep mozilla | wc -l
>> Renvoie les processus en cours avec mozilla dans le nom +  combien de fois il apparait
```

* Informer que un fichier est un script 
```bash
#!bin/bash
echo "Hello world"
echo $@
exit 0
```

# Execution

* Rendre executable fichier
```
chmod +x script.sh
```
* Lancer script
```bash
./script.sh arg1 arg2
```

* Si 0 pas d'erreur
* Si 1 erreur
* print = echo

* Variable : Jamais d'espace autour du "="
```script
chaine="exemple"
echo $chaine
```
* Operation arith (let=nombre)
```bash
let a=5
let a++
echo $a
>> 6
```
* Tableaux
```shell
tableau=(un deux trois)
echo ${tableau[2]}
>> trois
echo ${tableau[*]}
>>un deux trois
echo ${#tableau[*]}      #Nombre d'éléments
>>3 
```
* Arguments d'un script
```shell
${i} #Ième argument
$* #Tous les arguments
$# #Nombre d'arguments
```
* Saisie clavier (input)
```shell
read x y z ; echo $x $y $z

./script.sh 1 2 3
>> 1 2 3
```
* Expression numérique
```shell
x="67" ; ((x>0)) && echo $x est positif     #Condition sur 1 ligne : if x>0 then echo ...
>>67 est positif
``` 


# Variables
*  Variable résultat d'une commande
```shell
#!/bin/bash
user=$(whoami)
date=$(date)
echo $user
```
* Demander input à l'utilisateur
```
#!/bin/bash
echo "Votre prénom ?"
read prenom
```
* Générer chiffre aléatoire
```shell
#!/bin/bash
random=$(echo $RANDOM)
```
* Rendre variable accessible pour un autre script
```shell
#!/bin/bash
export var=$toto
```

# Conditions
```shell
#!/bin/bash

if [[ $var == "y" ]]; then
	echo "OK"
else
	echo "NOT OK"
fi
```
* Accéder au résultat de la ligne d'en haut avec `$?`
```shell
#/bin/bash
ping -c1 192.168.1.1
if (($? == 0)) ; then
	echo "Ping réussi"
fi 
exit 0
```
* 2
```shell
#/bin/bash
if find ~/local -name lparse -quit ; then
	echo "lparse trouvé dans l'arborescence ~/local"
fi 
exit 0
```

```shell
if instructions1 ; then     #Pas de conditions mais si une instruction vaut 0 alors on exec instruction
	instruction
elif instructions2; then
	instruction
else
	instructions
fi
```

```shell
#/bin/bash
if [ -s ${1} ] ; then
	echo "Fichier non vide"
fi 
exit 0
```
# Boucles
```shell
#!/bin/bash
while [[ $var != "y"]];
do
	echo "Again"
done
```

```shell
#!/bin/bash
for (i=0;i<10;i++);
do
	echo i
done

for x in 1 2 3 4 5;
do
	echo $x
```

# Opérations

# Fonctions
```shell
#!/bin/bash
function nomDuFonction {
	premiere commande
	deuxieme command
}
nomDuFonction (){
	premiere commande
	deuxieme command
}
nomDuFonction() { premiere commande; deuxieme commande; }

exempleDeFonction () {
	mkdir -p $1
	cd $1
}
```
* Exemple 
```shell
#!/bin/bash
fonctiondetest(){
   echo "Ma première fonction"
   echo $1
}
fonctiondetest
```
* Exemple 
```shell
#!/bin/bash
addition(){
   sum=$(($1+$2))
   return $sum
}
read -p "Entrez un numéro : " int1
read -p "Entrez un numéro : " int2
add $int1 $int2
echo "Le résultat est : " $?
```