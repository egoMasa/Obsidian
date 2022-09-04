# Guide Utilisateurs-Groupes Linux

## I) Groupes 

* Créer un groupe
```
(machine)root#sudo groupadd @groupe
```

* Ajouter utilisateur à un groupe
```
(machine)root#usermod -a -G @groupe @user
-usermod = Modifier un compte utilisateur
-a = append
-G = groupe
ou
(machine)root#gpasswd -a utilisateur groupe
```

* Ajouter utilisateur à plusieurs groupes 
```
(machine)root#usermod -a -G group1, group2, group3 @user

-usermod = Modifier un compte utilisateur
-a = append
-G = groupe
```

* Changer groupe principal d'un utilisateur
```
(machine)root#usermod -g @groupe @user
```

* Afficher groupes d'un utilisateur
```
(machine)root#groupes @user
```

* Supprimer utilisateur d'un groupe
```
(machine)root#gpasswd -d @nom_user @groupe

-gpasswd = Administre /group et /gshadow
-d = delete
```

* Supprimer un groupe
```
(machine)root#groupdel @groupe
```

## II) Utilisateur 
* Ajouter utilisateur + home
```
(machine)root#sudo adduser @user
ou
(machine)root#useradd -m -G @groupes -s /bin/bash @user
```

* Modifier password utilisateur
```
(machine)root#sudo passwd @user
```

* Modifier identifiant+home d'un utilisateur
```
(machine)root#usermod -m -d /home/new_home -l @new_user @user
```

* Suppression utilisateur
```
(machine)root#sudo deluser --remove-home @user
```
