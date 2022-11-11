# Guide VPN (Virtual Private Network)

## I) Principe et explication

* Un VPN est un réseau de communication privé qui utilise un réseau public (tel que l'internet) pour établir une connexion à distance
* Il crypte les données lors de l'envoi et les décrypte lors de la réception.
* Fournit une liaison spécialisée entre deux points sur l'internet
* L'équipement "VPN Concentrateur" créer la connexion VPN et gère l'acheminement des messages entre les ordinateurs et les dispositifs VPN. Il authentifie les utilisateurs, crypte les données, et attribue des adresses IP de tunnel aux utilisateurs. Néanmoins il ne sont pas obligatoire on l'utilise que pour les grosses entreprises. On peut utiliser des logiciels VPN sur d'autres équipements qui assure d'autres taches comme un firewall ou un routeur

## II) Types de VPN

### 1)Site-to-site
* Communication entre 2 entreprises via Internet
* Les données sont cryptés en entrée et décrypter en sortie

### 2)Host-to-site
* Communication entre client et une entreprise via Internet
* Les données sont cryptés après que le client soit connecter au VPN 

### 3)Host-to-Host
* Communication entre 2 clients via Internet
* Pas besoins d'équipement réseau supplémentaire mais juste un logiciel VPN

## III) Protocole VPN

### PPTP (Point-to-Point Tunneling Protocol)
* Le plus ancien protocol 
* Peu sécurisé 
* Obselète

### L2TP/IPsec (Layer2 Tunneling Protocol)
* Fonctionne par dessus IPsec (système de cléf)
* Système de multi-authentification +  sécurisation et du chiffrement des données grace à IPsec
* Possibilité d'un tunneling en mutli-threading et donc l'execution de plusieurs tâches sécurisées en simultané

### IKEv2 (Internet Key Exchange version 2)
* Basé sur IPsec (système de cléf)
* Identifie les deux bouts du tunnel IPsec
* On l'utilise souvent sur mobile grace à sa gestion du passage d'un réseau wifi peu sécurisée vers le réseau de données mobiles
### OpenVPN
* Le plus populaire
* Rapide et fiable
* Utilisent majoritairement AES 256 bits pour la méthode chiffrement
