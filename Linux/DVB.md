# Guide DVB (Télévision Numérique)

* Verifier si clef reconnue par noyau
```
cat /dev/dvb
```
* Afficher interface DVB
```
ip a
```
* Installer ***w-scan*** et ***dvb-apps***
```
apt-get install w-scan
apt-get install dvb-apps
```
* Lancer scan fréquences de chaines existantes (10min)
```
w_scan [options...] >> channels.conf
ou
time w_scan -ft > channels.conf 2> channels.log
```
* Accélerer le scan via fichier chaines locales
```
wget http ://10.30.3.1/safi/src/fr-Marseille

w_scan -X -ft -I fr-Marseille > channels.conf 2> channels.log
-X : pour zap/czap/xine ;
-M : pour mplayer ou mpv
```
* Regarder résultats scan ***/channels.zap***
```
cat /channels.zap
```
* Zapper sur la chaine ***Arte***
```
tzap -c channels.zap "ARTE(SMR6)"

! Arte peut être ammené à changer selon le resulat dans channels.zap
```
* Régler le tuner pour pouvoir regarde TV, Enregistrer, Manipuler flux, Diffuser flux sur réseau
* Installer paquet ***dvbsnoop*** (capture/analyse)
```
apt-get install dvbsnoop
```
* Installer paquet ***dvbsnoop***
```
apt-get install dvbsnoop
```
* Lister flux de facon séparés (selon PID)
```
dvbsnoop -pd 3 -s pidscan
```
* Lister PIDs 
```
dvbsnoop -s pidscan -pd @num
@num = niveau de détails
```
* Lister ***programme diffusés*** :
```
dvbsnoop -n 10 -pd 4 0x0
```
* Identifier les ***flux de transport***
```
dvbsnoop -n 10 -pd 4 0x10
```
* Obtenir renseignements sur ***services diffusés***
```
dvbsnoop -n 10 -pd 4 0x17
```
* Mesurer ***débit*** d'un PIB
```
dvbsnoop -s bandwidth -n 1000 -pd 2 @num
```
* Installer paquet ***mumudvb*** (diffusion)
```
apt-get install mumudvb
```
* Vérifier que ***serveur non actif*** (obligatoire)
```
pgrep -la mumudvb
```
* Créer fichier ***mumu0.conf*** et remplir
```
autoconfiguration=full
sap=1
card=0
freq=@LAFREQUENCEDEVOTREMULTIPLEX EN KHz
```
* Lancer mumudvb
```
mumudvb -d -c .mumudvbrc/mumu0.conf
```
* Adapter fichier de canaux au logiciel pour mplayer (regarder TV)
```
mkdir ~/.mplayer
w_scan -M -ft -I ../fr-Marseille >   ~/.mplayer/channels.conf 2> channels.log
```
### Compétences
* Montrer le périphérique dans l’arborescence des fichiers    
* Fichier channels.conf pour tzap ;
* Trouver la relation entre canal et fréquence TNT sur le site du csa [http://csa.fr](http://csa.fr/) 
- Vérifier la liste des canaux sur le site du csa [http://csa.fr](http://csa.fr/)  
- Sur le compte-rendu : liste des multiplex, fréquence, liste des chaînes.

