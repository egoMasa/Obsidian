
## Configuration 

* Activer routage ipv6
```
ipv6 unicast-routing
```
* Créer interface SVI
```
interface vlan XX
	ipv6 address ...
	no shutdown
```
* Activer ipv6 sur MLS
```
sdm prefer dual default
copy run startup-config
reload
```
