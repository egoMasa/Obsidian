* Tout est interdit par défaut (deny any any)
* Il faut appliquer la liste sur une interface en entrée ou sortie
* A partir du moment où l'on autorise certains trafics, tout le reste qui n'est pas renseigné se retrouve bloqué

## Configuration
* Créer ACL
```
ipv6 access-list NOM
	deny @protocol @source @destination
	permit @protocol @source @destination
```
* Application ACL sur interface {IN/OUT}
```
interface G0/0/0
	ipv6 traffic-filter NOM
```
# Exemple 
```cisco
ipv6 access-list BLOCK_HTTPS
	deny tcp any host 2001:DB8:1:30::30 eq www 
	deny tcp any host 2001:DB8:1:30::30 eq 443
	permit ipv6 any any
interface gigabitEthernet 0/1
	ipv6 traffic-filter BLOCK_HTTPS in
```