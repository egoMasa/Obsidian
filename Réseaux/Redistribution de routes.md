# Guide de redistribution de routes entre protocoles de routages

* Redistribuer les routes apprises par RIP dans OSPF
```
router ospf 1
	redistribute rip subnets
```
* Redistribuer les routes apprises par OSPF dans RIP
```
router rip
	redistribute ospf 1 metric 1
```
* Redistribuer les routes apprises par OSPF dans EIGRP
```
router eigrp 1
	redistribute ospf 1 metric 1
```
## Redistribution statique/defaut
```
router ...
	redistribute static
```