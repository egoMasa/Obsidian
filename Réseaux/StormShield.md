# Guide StormShield CSNA

* Connexion Firewall
```
http://10.0.0.254/admin
```
* Configuration système
```
Système/configuration/
>nom,langue,clavier
>heure (Synchronise)
Système/network/
>Proxy server 
>DNS
```
* Mot de passe admin
```
CONFIGURATION/Système/Administrateurs
```
* Sauvegarder une configuration
```
CONFIGURATION/SYSTEME/MAINTENANCE/BACKUP
>Configuration automatic backup 
```
* Traces et logs
```
NOTIFICATION/LOGS-SYSLOG
>ON
LOG/NETWORKTRAFFIC
>ACTIONS,EXPAND
```
* Objets (definition interface,hosts)
```
OBJECTS/NETWORK OBJECTS/
>add
```
* Configuration interface
* Routage statique
* Routage dynamique
* DNS
* DHCP
* NAT 
* PAT
* Filtrage
* Protection : proxy, HTTPS, analyse,
* Authentification : annuaire, utilisateurs, filtrage accès