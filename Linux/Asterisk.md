# Guide Asterisk Linux

# 0) Installation paquets
* Installer paquet asterisk et asterisk-mobile
```
sudo apt-get install asterisk
sudo apt-get install asterisk-mobile
```
* Réinstaller totalement asterisk
```
apt purge asterisk
apt purge asterisk-config
```

# 1) Fichier de configurations
* Vider fichier de tous commentaire
```
sed -e 's/;.*$//' \
sed -e 's/\t/ /g' \
sed -e 's/ */ /' \
sed asteriskconf/sip.conf | uniq > /etc/asterisk/sip.conf
echo > fichier.conf
```
* Les fichiers de configurations sont : 
	* ***sip.conf*** : Utilisateurs enregistrés
	* ***extensions.conf*** : Instructions 
	* ***chan_mobile.conf***
	
 * Le fichier de logs : ***/var/log/asterisk/messages***

* Lancer asterisk 
```
asterisk -rvvv
```

# Ajout d'utilisateur
* Editez le fichier ***sip.conf***
```
[identifiant_client]
type=friend
secret=azerty
host=dynamic  #Unspecified
context=default
```
* Observer communication client vers le serveur asterisk
```
tshark -n -i eth0 -f "port sip"
```
* Afficher téléphones configurés dans sip.conf
```
asterisk -rvvv
>>sip show peers
>>sip show user
>>sip show friend
```
* Exemple de configuration
```
[general]
context=public
allowoverlap=no
udpbindaddr=0.0.0.0
tcpenable=no
tcpbindaddr=0.0.0.0
transport=udp
srvlookup=yes

[57i]
type=friend
secret=MonSecret
host=dynamic
context=default

[gs1]
type=friend
secret=mdp
host=dynamic
```

# Appeler vers un téléphone
* Ajouter instructions dans la section ***default*** du fichier ***extensions.conf***
```
[default] #Utilisé par la console asterisk
exten => 701,1,Dial(SIP/IDENTIFIANT_CLIENT,42)

#exten => : Désigne le numéro du poste
#701 : numero du téléphone (lettres,carac,chiffre)
#1 : Priorité
#Dial : (PROTOCOLE/CANAL,duree,param)
```
* Activer le canal oss dans fichier de configurations des modules
```
nano module.conf
load => chan_oss.so #sous global
```
* Passer l'appel
```
asterisk -rvvvv
>>console dial 701
```
* Raccrocher l'appel
```
asterisk -rvvvv
>>console hangup
```
* Exemple de configuration
```
[general] #Paramètres
static=yes
writeprotect=no
clearglobalvars=no

[globals] #Variables globales
CONSOLE=Console/dsp

[default] ;la section par defaut, c'est celle de la console

exten => 701,1,Dial(SIP/gs1,42)
```