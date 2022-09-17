# Guide sauvegarde Linux
## I) Commande rsync

* Sauvegarde de fichier classique
```bash
(machine)root#rsync source/ destination/
```
* Sauvegarde de fichiez via SSH
```bash
(machine)root#rsync -az source/ login@serveur.org:/destination/
ou
(machine)root#rsync -avz chemin/source/ -e "ssh -p port" user@ip:"/chemin/de destination avec espaces/"
```