
# Guide annuaire LDAP

# 0) Paquets 
* Installer
```
sudo apt-get install ldap
sudo apt-get install ldap-utils
sudo apt-get install slapd
```

# 1) Création d'un annuaire
* Installer paquet slapd
```
sudo apt-get install slapd
```
* Reconfigurer les paramètres du serveur installé
```
dpkg-reconfigure slapd
>> il ne faut pas omettre la configuration : NON
>> Nom de domaine : osaka.rtlum.
>> L’organisation : Osaka.
>> Le mot de passe administrateur : au choix
>> Le module de base de données à utiliser sera MDB.
```
* Se connecter au serveur LDAP pour vérifier sa création
```
ldapsearch -x -b dc=osaka,dc=rtlum -H ldap://serveur
```
* Installer paquets outils ldap
```
apt-get install ldap-utils
```
* Il faut créer un fichier que l'on va envoyé sur le serveur
* Création de la structure avec class people et groups
```
>touch struct.ldif
>nano struct.ldif

dn: ou=People,dc=osaka,dc=rtlum
objectClass: organizationalUnit
ou: People

dn: ou=Groups,dc=osaka,dc=rtlum
objectClass: organizationalUnit
ou: Groups
```
* Envoyer fichier de structure (struct.ldif)
```
ldapadd -c -x -D cn=admin,dc=osaka,dc=rtlum -f struct.ldif -W
```

# 2) Gestion utilisateur

## Ajout d'un utilisateur
* Créer un fichier de création d'un utilisateur
```
touch user.ldif
```
* Remplir fichier, ici pour créer l'utilisateur Gandalf
```
dn: uid=gandalf,ou=People,dc=osaka,dc=rtlum
uid: gandalf
cn: gandalf
objectClass: account
objectClass: posixAccount
objectClass: top
objectClass: shadowAccount
userPassword: {crypt}$6$rlWmT3BJ$ZqG...longue chaîne chiffrée
shadowLastChange: 17351
shadowMax: 99999
shadowWarning: 7
loginShell: /bin/bash
uidNumber: 3001
gidNumber: 3000
homeDirectory: /users/magic/gandalf
gecos: Gandalf The Grey, directeur secteur magie
```
* Envoie de l'utilisateur sur serveur
```
ldapadd -c -x -D cn=admin,dc=osaka,dc=rtlum -f gandalf.ldif -W
```

## Modification d'un utilisateur
* Modifier paramètres d'un utilisateur, on change gecos
```
dn: uid=gandalf,ou=People,dc=osaka,dc=rtlum
changetype: modify
replace: gecos
gecos: Gandalf The White, directeur secteur magie
```
* Envoie des modification sur serveur
```
ldapadd -c -x -D cn=admin,dc=osaka,dc=rtlum -f modif.ldif -W
```

## Suppression d'un utilisateur
* Supprimer utilisateur
```
dn: uid=gandalf,ou=People,dc=osaka,dc=rtlum
changetype: delete
```
* Envoie des modification sur serveur
```
ldapadd -c -x -D cn=admin,dc=osaka,dc=rtlum -f del.ldif -W
```

# 3) MAJ et Sauvegarde
* On peut sauvegarder la VM ou le serveur slapd
* On va snapshot la VM et sauvegarder l'image
* Sauvegarder la configuration sur le serveur slapd
```
service sldapd stop
slapcat -n 0 > backupLDAPconfig.ldif
slapcat -n 1 > domaine.ldif
service sldapd start
```
* Reconstruire l'image
```
service sldapd stop
slapadd -F /etc/ldap/slapd.d -n 0 -l backupLDAPconfig.ldif
slapadd -F /etc/ldap/slapd.d -n 1 -l domaine.ldif
service sldapd start
```
