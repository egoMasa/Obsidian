# 1) C'est quoi STP
* STP est un protocole de commutation qui intervient sur la couche 2 du modèle OSI (Liaison de données)
* Il concerne uniquement les SW (2 ou 3)
* Il concerne les trames et non les paquets 

# 2) A quoi sert STP
* STP sert à limiter les boucle de commutation et s'assurer que le chemin le plus rapide est pris
* Les trames ne possède pas de TTL donc elle sont potentiellement intuable ce qui peut arriver avec une boucle de communication.
* Si un broadcast est envoyé sur une boucle les SW vont finir par crash car ils seront submergés infiniment de trames qui ne finiront jamais.
* Sans STP d'activer on peut sans s'en rendre compte prendre un chemin plus long est lent et laisser le chemin idéale inactif

# 3) Comment fonctionne STP
* STP est activer automatiquement sur chaque SW, c'est pour cela qu'on attend souvent 30s avant que le réseau soit utilisable correctement (port orange sous PKT)
## Etape 1 : Désignation pont racine/root (SW root)
* Les SWs vont pendant ces 30s s'envoyer tout un tas de Trames (BPDU)afin qu'il puisse définir qui sera le SW root, pour cela plusieurs critères sont mis en place :
	* 1 - Priorité du SW la plus élevée, sinon
	* 2 - Adresse MAC la plus faible

## Etape 2 : Désignation ports racine/root(RP) (saut en face du root)
* Les ports root désigne par les ports le plus proche du SW root, en réalité il s'agit toujours des ports adjacents/ports saut suivant du routeur root.
* Chaque switchs possèdent un port root qui lui permet de remonter à la racine.
* Si un SW est face à deux SW reliés au routeur, il va choisir selon :
	* 1 - Cout liaison (cost)
	* 2 - Priorité la plus élévée
	* 3 - MAC la plus faible

## Etape 3 : Désignation ports désignés(DP)
* Les ports désignes désigne par quel port unique le traffic va passer un segment précis.
* Il faut un DP par segment
* Le SW root a que des ports désignés car c'est la racine et forcement c'est lui qui envoie tout.
* Si deux SW possèdent un cout équivalent, des critères sont mis en place :
	* 1 - Priorité la plus haute
	* 2 - MAC la plus faible

## Etape 4 : Désactivation ports non désignés
* STP va désactiver les autres câbles non utilisés qui se réactiveront uniquement en cas de panne du chemin actuelle.

## Précision sur élection 
* De base chaque SW se désigne root et le transmet aux autres. Ils se mettent d'accord en communiquant entre eux sur lequel le sera vraiment (priorité sinon MAC). Ce root ne sera plus remis en cause

# 4) Comment on configure STP
* Désigner un SW root par VLAN (SW1->VLAN 10, SW2->VLAN20)
```cisco
SW1(config)#spanning-tree vlan 10 priority 4092
SW1(config)#spanning-tree vlan 20 priority 8192

SW2(config)#spanning-tree vlan 20 priority 4092
SW2(config)#spanning-tree vlan 10 priority 8192
```
# 5) STP avec VLANS
* Il est recommande de désigné un SW root par vlan, si on a 50 vlans et 2 SW, chacun va être root de 25 vlans.

# 6) Attaque potentiel
* On peut faire une attaque via un SW préconfiguré avec priorité faible pour communiquer qu'on devient le root SW et ainsi faire passer chaque trames par nous.
* Pour contrer ca on peut configurer certaines interfaces switch coté vers PC en les mettant en PDU Block (passive interface) et empêcher que si un SW se rejoins qu'il modifie l'architecture (on négocie plus.)