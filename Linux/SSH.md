# Guide SSH Linux

## I) Principe et explication

* SSH (pour _Secure SHell_) désigne à la fois un protocole et un logiciel permettant une connexion à distance sécurisée. Son implémentation la plus connue et la plus utilisée, _OpenSSH_, remplace avantageusement des outils et protocoles historiques comme telnet, rcp, ou ftp.

* Un outil connu utilisé conjointement avec SSH est `rsync`, qui permet de faire de la synchronisation unidirectionnelle entre deux fichiers ou arborescences de fichiers, locaux mais aussi distants.

## II) Utilisation 
* Connexion SSH
```bash
(machine)root#ssh login@{machine|IP}
```
* Changer d'utilisateur en SSH
```bash
(machine)root#login @nom
```
* Connexion SSH
```bash
(machine)root#ssh login@{machine|IP}
```
* Connexion SSH avec ports
```bash
(machine)root#ssh login@{machine|IP} -p @port
```

## III) Installation SSH 

### Coté serveur
* Installer paquet ***openssh-server***
```bash
(machine)root#apt-get install openssh-server
```
* Verifier son fonctionnement
```bash
(machine)root#systemctl status ssh
```

### Coté client
* Installer paquet ***openssh-client***
```bash
(machine)root#apt-get install openssh-client
```
* Verifier son fonctionnement
```bash
(machine)root#systemctl status ssh
```

## IV) Authentification via cléfs
* Générer paire de cléfs RSA sur client
```bash
(machine)root#ssh-keygen -t rsa 
```
* Envoyez cléf publique vers destination serveur
```bash
(machine)root#scp @chemin_clef @ip_desti:chemin
```
* Ajouter cléf publique dans le fichier ***.ssh/authorized_keys*** serveur
```bash
(serveur)root#mv @clef_pub .ssh/authorized_keys
```

## V) Configuration client SSH
* Simplifier connexion SSH
```bash
host @nom_pour_cmd
	hostname @ip_cible
	user @login
	port @port
	localForward @port_libre @port_cible
	#localForward 8080 thorin:80
```

* Connexion simplifiée
```bash
(machine)#ssh @nom_pour_cmd
```