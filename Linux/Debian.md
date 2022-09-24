# Guide Debian Linux
## I) Installation station
* Télécharger image debian
```bash
(machine)root#wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.4.0-amd64-netinst.iso
```

-   Créer l’image de la machine 

```bash
(machine)root#qemu-img create -f qcow2 @nom.qcow2 @?G
```
-   Lancer l’installation 
```bash
(machine)root#kvm -m 1000 -hda @nom.qcow2 -cdrom @nom.iso
```

@nom.qcow2 = disque créer à la commande précedente

@nom.iso = image téléchargée

## II) Nom de machine 
-   Commande hostnamectl 
```bash
(VM)root#hostnamectl set-hostname @nouveau_nom
```
-   Redémarrer machine 
```bash
(VM)root#systemctl reboot
```

## III) Paquets complémentaires
```bash
(VM)root#sudo apt install make gcc dkms linux-source linux-headers-$(uname -r)
```
