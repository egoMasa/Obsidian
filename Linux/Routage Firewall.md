# Guide Routage/Firewall

## I) Routage
### Equipement 
* Routeur-Firewall : Station physique
* Ordi interne : VM sur SW ***vde2***
* Ordi externe : Station Osaka-HongKong

### Commandes:
* Activer routage (provisoire)
```
echo 1 > /proc/sys/net/ipv4/ip_forward
ou
sysctl -w net.ipv4.ip_forward=1
```
* Desactiver routage (provisoire)
```
echo 0 > /proc/sys/net/ipv4/ip_forward
ou
sysctl -w net.ipv4.ip_forward=0
```
* Activer routage dans fichier de configuration ***/etc/sysctl.d/routeur.conf***
```
net.ipv4.ip_forward=1
```
* Ajouter une route pour VM
```
ip route add @reseau_cible via @ip_saut
```


## II) Firewall
* Installer ***firewalld***
```
apt-get install firewalld
```
* Gérer firewalld via ***firewall-cmd***
```
apt-get install firewalld
```
* Montrer politiques DROP,REJECT,ACCEPT via Tshark
```
tshark -n
```
* Autoriser un port
```
firewall-cmd --add-port=22/tcp
ou
firewall-cmd --add-port=22/tcp --permanent
firewall-cmd --reload
```
* Autoriser une adresse destination
```
```
* Lister zones 
```
firewall-cmd --get-zones
firewall-cmd --list-all-zones
firewall-cmd --info-zone @nom_zone
```
* Créer et supprimer des zones
```
firewall-cmd --new-zone=@nom --permanent
firewall-cmd --delete-zone=@nom --permanent
```
* Zone par défaut 
```
firewall-cmd --get-default-zone
firewall-cmd --set-default-zone=@nom
```
* Activer la masquerade (mode external)
```
```
* Changer adresse et port de destination
```
firewall-cmd --zone=external --add-forward-port=port=:proto=:toaddr=:toport= [--permanent]

ex port HTTP vers l'IP 172.21.21.10

firewall-cmd --zone=external --add-forward-port=port=80:proto=tcp:toaddr=172.21.21.10:toport=80 --permanent

```