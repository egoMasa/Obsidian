# Guide Analyse réseau

## I) Tshark
* Installer ***tshark***
```
sudo apt-get install tshark
```
* Afficher traffic sur toutes les interfaces
```
tshark -n -i any
```
* Afficher traffic sur une interface 
```
tshark -n -i @interface
```
* Filtrer un port
```
tshark -n -i @interface -f "port XX"
```
* Filtrer un host
```
tshark -n -i @interface -f "host X.X.X.X"
```
* Filtrer un port et un host
```
tshark -n -i @interface -f "port XX and host X.X.X.X"
```
* Filtrer un port et pas un host
```
tshark -n -i @interface -f "port XX and not host X.X.X.X"
```

## II) Ping et envoie de données 
* Envoyer donnée sur un port cible 
```
nc @host @port
```

## IV) Scan ports ouverts (nmap, ss)
* Afficher ports ouverts sur sa machine
```
ss -tunlp
```
* Afficher ports ouverts sur une machine cible
```
nmap @ip
```