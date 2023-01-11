# Guide SSH Cisco

* Activer cryptage des mots de passe
```
service password-encryption
```
* Création d'un utilisateur avec mot de passe et privilèges
```
username @nom password @mp  
```
* Authentification des utilisateurs locaux dans les consoles distantes :
```
(config)#line vty 0 4
(config-line)#login local
(config-line)#transport input ssh
```
* Protection de ports
```
int G0/0
	switchport port-security violation {protect | restrict | shutdown}
	switchport port-security mac-address sticky
```