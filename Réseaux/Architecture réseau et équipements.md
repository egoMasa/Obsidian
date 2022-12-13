# Guide Architecture réseau et équipements

## I) LAN/WAN

* Quand on parle de réseau, on vient vite à parler de pleins de réseaux différents avec des caractéristiques propres. La grandeur d’un réseau est le critère le plus important, c’est pour cela que plusieurs types de réseaux sont définis afin de savoir de quoi parle vraiment.

 * **Réseau LAN*** (Local Area Network = Réseau local avec une place géographique précise. Débit plus élevé
![[Pasted image 20220901123300.png]]
* ***Réseau WAN***(Wide Area Network = Réseau étendu qu’on appelle aussi souvent Internet. WAN est généralement utilisé pour décrire tout réseau auquel nous nous connectons et qui existe au-delà de notre routeur. Et cela, peu importe que le serveur se trouve à un kilomètre ou sur un autre continent. A tendance a avoir un débit plus faible due au nombre plus élevé de transmissions.
![[Pasted image 20220901123521.png]]

 


## II) Equipements réseau

* Routeurs

Le routeur est l’équipement réseau par excellence sans eux rien de ne passe où transite sur un réseau, son but est d’orienter les données à travers un réseau. Il intervient sur la couche réseau (3) du modèle OSI-TCP/IP. Il est souvent appelé “passerelle par défaut” car c’est à lui que l’on va transférer nos paquets et qui nous permettra de quitter notre réseau local. C’est lui qui relie différents réseaux entre eux.

* Switchs

Le switch est un équipement qui n’intervient pas dans la couche réseau, son but est de mettre plusieurs ordinateurs en contact de façon intelligente. Il transmet les données seulement aux personnes concernées en se basant sur les adresses MAC de la couche 2. Il possède un nombre de ports permettant de connecter plusieurs appareils.

![[Pasted image 20220901123814.png]]

## III) Topologie réseau

### Topologie physique
* Il s’agit de la structure physique de votre réseau. C’est donc la forme, l’apparence du réseau. Il existe plusieurs topologies physiques :

* ***1)le bus*** =  tous les ordinateurs sont connectés entre eux par le biais d’un seul câble réseau débuté et terminé par des terminateurs. La vitesse de transmission est très faible. On utilise une méthode d’accès appelée CSMA/CD. Avec cette méthode, une machine qui veut communiquer écoute le réseau pour déterminer si une autre machine est en train d’émettre. Si c’est le cas, elle attend que l’émission soit terminée pour commencer sa communication. Sinon, elle peut communiquer tout de suite.
![[Pasted image 20220901124125.png]]
* 2)***l’étoile*** (la plus utilisée) = N’importe quel appareil (routeur, commutateur, concentrateur, …) peut être au centre d’un réseau en étoile. il n’y a pas de risque de collision de données
![[Pasted image 20220901124222.png]]
- 3)***le mesh*** (topologie maillée) = On relie tous ensemble mais ça coûte extrêmement cher ! On ne l’utilise jamais

- 4)***l’anneau*** = Pareil que le bus mais mis en rond avec un système de jeton pour communiquer ce qui évite les collisions

- 5)***hybride*** = Mixte de différentes topologie qui communique entre elles. Elle varie selon les besoins

### Topologie logique

* Une topologie logique est la structure logique d’une topologie physique, c’est-à-dire que la topologie logique définit comment se passe la communication dans la topologie physique

❗L’une (topologie physique) définit la structure physique (l’apparence physique, la forme) de votre réseau, l’autre (topologie logique) définit comment la communication se passe dans cette forme physique.

