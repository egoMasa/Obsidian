# Guide MongoDB

* Lancer VM 
```
kvm -m 5G -smp 8 -hda debian-mysql.qcow2 -k fr -net nic,macaddr=42:01:02:03:04:05 - net vde,sock=/var/run/vde2/kvmtap0.ctl -usb -device usbhost,vendorid=0x18d1,productid=0x4ee7
```
* Installer mariadb-server
```
sudo apt update
sudo apt-get install mariadb-server
sudo systemctl status mariadb.service
```
* Installer mariadb-server
```
sudo apt update
sudo apt-get install mariadb-server
```
* Se connecter à la base de données
```
sudo mysql
```
* Modifier password de la base de données
```
> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('azerty');
> FLUSH PRIVILEGES;
> QUIT;
```
* Se connecter en root à la BD
```
mysql -u root -p
> show databases;
```
* Créer une BD
```
mysql -u root -p
> CREATE TABLE etudiant
> show databases;
```
* Créer un nouvel utilisateurs
```
mysql -u root -p
> CREATE USER 'paul'@'%' IDENTIFIED BY 'looking4you';
> GRANT ALL ON etudes.etudiant TO 'paul'@'localhost';
		SELECT
		UPDATE
		DELETE
		INSERT
> CREATE USER 'toto'@'IP_machine' IDENTIFIED BY 'azerty';
> GRANT ALL PRIVILEGES ON etudiant.* TO 'toto'@'IP_machine' WITH GRANT OPTION;


> FLUSH PRIVILEGES
> show databases
```
* Supprimer un utilisateur
```
mysql -u root -p
> DROP USER 'toto'@'IP_machine'
```
* Saisir une table de la bd
```
mysql -u root -p
> SHOW DATABASES;
> USE etudiant;
>	SHOW TABLES
>	DESCRIBE table_etudiant
```