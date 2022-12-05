
# 1) OSPF
* Echange entre routeurs via ***link-local*** 
* Multicast OSPF routers : ***FF02::5***
* Multicast Designated routers : ***FF02::6***
* Configuration
```cisco
ipv6 unicast routing
ipv6 router ospf 1
	router-id 1.1.1.1
	
int G0/0/0
	ipv6 address 2001:db8:cafe::1/64
	ipv6 ospf 1 area 0
	ipv6 ospf priority 20
	ipv6 ospf cost 20

	
sh ipv6 route ospf
sh ipv6 ospf neighbor
sh ipv6 int brief
```
* Multizone OSPFv3
```
ipv6 unicast-routing
ipv6 router ospf 1
	router-id X.X.X.X
int G0/0
	ipv6 ospf 1 area @area_reseau_voisin_interface
```
* Commande de vérification
```
show ipv6 ospf
show ipv6 route
show ipv6 ospf database
show ipv6 ospf interface
show ipv6 ospf neighbor
```
* Résumé de route (à l'intérieur du processus, en multizone seulement)
```
ipv6 router ospf 
	area X range @route_resumée
```
# 2) EIGRP
* Adjacences via ***link-local*** 
* Groupe multicast : ***FF02::A***
* Shutdown de base, il faut l'activer
* Configuration
```cisco
ipv6 unicast routing
ipv6 router eigrp 1
	eigrp router-id 1.1.1.1
	no shutdown
	
int @int
	ipv6 adress 2001:db8:cafe::1/64
	ipv6 eigrp 1
	ipv6 summary-address eigrp ASN PREFIX/MASQUE
	
sh ipv6 route eigrp
sh ipv6 eigrp neighbor
sh ipv6 int brief

```
# 3)RIPnG
* Groupe de multicast : ***FF02::9***
```cisco
ipv6 unicast routing
ipv6 router rip @nom

int @int
	ipv6 adress 2001:db8:cafe::1/64
	ipv6 rip @nom enable
	ipv6 rip NOM summary-address PREFIX/MASQUE
	
sh ipv6 route rip
sh ipv6 rip protocols
sh ipv6 int brief

```