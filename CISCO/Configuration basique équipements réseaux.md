
# I)Lancement système (boot system)

* La commande principale
```
S1(config)#boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin
```
* Périphérique de stockage
```
flash:
```
* Le chemin d'accès au système de fichiers
```
c2960-lanbasek9-mz.150-2.SE/
```
* Le nom du fichier IOS
```
c2960-lanbasek9-mz.150-2.SE.bin
```
* voir sur quoi le fichier de démarrage IOS actuel est réglé
```
show boot
```

## Récupération après panne
* Afficher le chemin d'accès de la variable d'environnement du commutateur
```
switch: set
```
* Initialisez le système de fichiers flash
```
switch: flash_init
```
* Visualiser les répertoires et les fichiers en flash
```
switch: dir flash:
```
* Modifier le chemin de la variable d'environnement BOOT que le commutateur utilise pour charger le nouvel IOS en flash
```
switch:BOOT=flash:c2960-lanbasek9-mz.150-2.SE8.bin
```
* Vérifier le nouveau chemin de la variable d'environnement BOOT
```
switch: set
```
* Charger le nouvel IOS
```
switch : boot
```

# II) Configuration SWITCHS

## II.1)SVI

### Etape 1 : Configurer l'interface de gestion
* Passer en mode de configuration d'interface pour SVI.
```
S1(config)#interface vlan 99
```
* Configurer l'adresse IPv4 de l'interface de gestion.
```
S1(config-if)#ip address 172.17.99.11 255.255.255.0
```
* Configurer l'adresse IPv6 de l'interface de gestion.
```
S1(config-if)#ipv6 address 2001:db8:acad:99::1/64
````
* Activer l'interface de gestion.
```
S1(config-if)#no shutdown
````
* Enregistrer la configuration en cours dans la configuration de démarrage.
```
S1#copy running-config startup-config
```
### Etape 2 : Configurer la passerelle par défaut
* Configurer la passerelle par défaut pour le commutateur.
```
S1(config)#ip default-gateway 172.17.99.1
```
### Etape 3 : Vérifier la configuration
* Déterminer l'état des interfaces physiques et virtuelles
```
S1#show ip interface brief
S1#show ipv6 interface brief
```

## II.2)Couche physique
* Passer en mode de configuration d'interface.
```
S1(config)#interface FastEthernet 0/1
```
* Configurer le mode bidirectionnel d'interface.
```
S1(config-if)#duplex full
```
* Configurer la vitesse d'interface
```
S1(config-if)#speed 100
```
* Enregistrer la configuration en cours dans la configuration de démarrage.
```
S1#copy running-config startup-config
```
## II.3) Commandes de vérifications
  Afficher l'état et la configuration des interfaces.
```
S1#show interfaces [_interface-id_]
```
* Afficher la configuration initiale actuelle.
```
S1#show startup-config
```
* Afficher la configuration courante.
```
S1#show running-config
```
* Afficher les informations sur le système de fichiers Flash.
```
S1#show flash
```
* Afficher l'état matériel et logiciel du système.
```
S1#show version
```
* Afficher l'historique de la commande saisie.
```
S1#show history
````
* Afficher les informations IP d'une interface.
```
S1#show ip interface [_interface-id_]
S1#show ipv6 interface [_interface-id_]
```
* Afficher la table d'adresses MAC.
```
S1#show mac-address-table
S1#show mac address-table
```

# III) Connexion à distance (SSH)
### Etape 1 : Vérifier le support SSH
* Vérifier que le commutateur supporte SSH
```
S1# show ip ssh
```
### Etape 2 : Configurer le domaine IP
* Configurez le nom de domaine IP du réseau 
```
S1(config)# ip domain-name cisco.com
```
### Etape 3 : Générer des paires de clés RSA
* Configurez le nom de domaine IP du réseau
```
S1(config)#crypto key generate rsa
```
### Etape 4 : Configurer l'authentification des utilisateurs
* Configurer l'authentification des utilisateurs
```
S1(config)#nom d'utilisateur admin secret ccna
```
### Etape 5 : Configurer les lignes de vty
* Exiger une authentification locale pour les connexions SSH à partir de la base de données locale des noms d'utilisateur.
```
S1(config)#line vty 0 15 
S1(config-line)#transport input ssh 
S1(config-line)#login local 
S1(config-line)#exit
```
### Etape 6 : Activer SSH version 2
* Activez la version SSH
```
S1(config)# ip ssh version 2
```

# IV) Configuration routeur
## IV.1)Paramètres de base (nom,pw,encryp)
```
Router#configure terminal 
Router(config)#hostname R1 
R1(config)#enable secret class 
R1(config)#line console 0 
R1(config-line)#password cisco 
R1(config-line)#login 
R1(config-line)#exit 
R1(config)#line vty 0 4 
R1(config-line)#password cisco 
R1(config-line)#login 
R1(config-line)#exit 
R1(config)#service password-encryption
```
* Configurez une bannière pour fournir une notification légale d'accès non autorisé
```
R1(config)#banner motd #Authorized Access Only!#
```
* Enregistrez les modifications sur un routeur
```
R1#copy running-config startup-config
```

## IV.2)Interfaces routeur
```
R1(config)#interface gigabitethernet 0/0/0 
R1(config-if)#ip address 192.168.10.1 255.255.255.0 
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64 
R1(config-if)#description Link to LAN 1 
R1(config-if)#no shutdown 
R1(config-if)#exit 
R1(config)#interface gigabitethernet 0/0/1 
R1(config-if)#ip address 192.168.11.1 255.255.255.0 
R1(config-if)#ipv6 address 2001:db8:acad:2::1/64 
R1(config-if)#description Link to LAN 2 
R1(config-if)#no shutdown 
R1(config)#interface serial 0/0/0 
R1(config-if)#ip address 209.165.200.225 255.255.255.252 
R1(config-if)#ipv6 address 2001:db8:acad:3::225/64 
R1(config-if)#description Link to R2 
R1(config-if)#no shutdown
```
## IV.2)Interfaces loopback
```
Router(config)#interface loopback_number_ 
Router(config-if)#ip address _ip-address subnet-mask_
```
## IV.3)Vérification configuration
* Vérification interface routeur
```
R1#show ip interface brief
R1#show ipv6 interface brief
```
* Vérifier les adresses locales et multidiffusion des liens IPv6
```
R1#show ipv6 interface gigabitethernet 0/0/0
```
* Vérification de la configuration d'interface
```
R1 show running-config interface gigabitethernet 0/0/0
```
* Vérification des routes
```
R1#show ip route
R1#show ipv6 route
```
