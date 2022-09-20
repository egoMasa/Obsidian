# Guide Modems

## I) Connexions physique
* Numéro de chaque modem
```
Dale = 912 (Appelant)
Chip = 911 (Appelé)
```
* Télécharger ***minicom***
```
apt-get install minicom
```
* Lancer minicom sur device correcte 
```
minicom -D /dev/ttyS0
```
* Changer protocoles modem
```
ATN1 = Version 21
ATN2 = Version 22
```
* Remettre état d'usine
```
ATZ3
```
* Afficher paramètres modem
```
ATI0
ATI1
```
* Lancer appel d'un modem vers l'autre (appelant)
```
ATDT@numero_appelé
```
* Répondre à l'appel (appelé)
```
ATA
```
## II) Connexion via réseau

### Cote serveur 

* Installer paquet ***mgetty***
```
apt-get install mgetty
```
* Lancer ***mgetty***
```
mgetty ttyS0 -D /dev/ttyS0
```
* Surveiller activités sur port serie ***ttyS0***
```
tail -f /var/log/mgetty/mg_ttyS0.log
```
* Installer paquet ***ppp*** (point-to-point protocol)
```
apt-get install ppp
```
* Ajouter utilisateur ***ppp*** et ***dale*** au groupe ***dip***
```
adduser ppp
adduser dale
usermod -a ppp -G dip
usermod -a dale -G dip
```
* Créer fichier ***/etc/systemd/system/mgetty.service***
```
touch /etc/systemd/system/mgetty.service
```
* Ajouter lignes dans ***/etc/systemd/system/mgetty.service***
```
[Unit]
Description=Smart Modem Getty(mgetty)
Documentation=man:mgetty(8)
Requires=systemd-udev-settle.service
After=systemd-udev-settle.service

[Service]
Type=simple
ExecStart=/sbin/mgetty -D -s 115200 /dev/ttyS0
Restart=always
PIDFile=/var/run/mgetty.pid.ttyS0

[Install]
WantedBy=multi-user.target
```
* Recharger la ***configuration*** :
```
systemctl daemon-reload
```
* Lancez le ***service*** :
```
service mgetty start
```
* Lancez le ***service*** :
```
service mgetty start
```
* Éditez ***/etc/ppp/options***
```
-detach
asyncmap 0
netmask 255.255.255.0
proxyarp
lock
crtscts
auth
+pap
usehostname
modem
debug
```
* Éditez le fichier ***/etc/ppp/pap-secrets***
```
@nom_client * @mp *
@nom_serveur * @mp *
```
* Éditez le fichier ***/etc/mgetty/login.config***
```
timeout 20
/AutoPPP/      -   ppp     /usr/sbin/pppd auth -chap +pap login
ppp            -   a_ppp   /usr/sbin/pppd auth -chap +pap login
*              -   -       /bin/login @
```
* Éditez ***/etc/ppp/options.ttyS0 ***
```
192.168.100.1:192.168.100.4
modem
crtscts
silent
proxyarp
```

### Cote client 

* Installer paquet ***ppp***
```
apt-get install ppp
```
* Éditez le fichier ***~/ppp***
```
#!/bin/sh
/usr/sbin/pppd connect '/usr/sbin/chat -V -f /home/stud/ppp.script' /dev/ttyS0 57600 -detach crtscts modem defaultroute idle 1800 debug
```
* Éditez le fichier ***~/ppp.script***
```
'' ATZ OK ATM0 OK ATDT@num_serveur 'CONNECT' '' ogin: ppp
```
* Editez le fichier ***/etc/ppp/pap-secrets*** à la fin
```
chip * toto *
dale * toto *
```
* Lancer script de connexion
```
sudo ./ppp
```
*  Activer interfaces ***ppp0*** sur les 2 machines, relevez leurs adresses IPs
```
watch ip a
```
* Utiliser ***Tshark*** sur l'interface ***ppp0***
```
tshark -n -i ppp0
```
* Ping depuis serveur vers client et inversement
```
ping @ip
```
* Afficher état table de routage
```
ip route
```