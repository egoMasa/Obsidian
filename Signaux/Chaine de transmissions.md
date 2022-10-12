# Guide chaîne de transmission

# Rappel 
* Spectre d'un signal = Domaine fréquentielle grâce à la TF/SF, représentation sous forme de raie de fréquences avec amplitude
* On utilise la valeur absolue pour representrer un spectre car physiquement les fréquences négatives n'existent pas
* Pour retrouver fréquence à partir de l'équation
```
A*cos({3,8*10^6}*t)
f = {3,8*10^6}/2*pi
```

# Modulation
* Un signal modulant (faible fréquence)
`m(t) = Am*cos(2*pi*fm*t+phi)`
* Un signal porteur (haute fréquence)
`p(t) = Ac*cos(2*pi*fc*t+phi)`
* Un signal modulé (résultat)
`s(t) = m(t).*p(t)`
* Astuce : si m(t) et p(t) sont des cos ou sin on peut utiliser les formules de ces dernières pour linéariser et analyser plus facilement
```
cos(a)*cos(b) = {cos(a-b)+cos(a+b)}/{2}
sin(a)*sin(b) = {sin(a-b)-sin(a+b)}/{2}
sin(a)*cos(b) = {sin(a+b)+sin(a-b)}/{2}
cos(a)*sin(b) = {sin(a+b)-sin(a-b)}/{2}
```


# Modulation AM
* Variation de l'amplitude 
* Signal modulant sur enveloppe du signal modulé
* Taux de modulation m :
```
m = Am/(Ap/2)
m = sqrt(({pt/pc}-{1})*2)
m = {A-B}/{A+B}
```
![[Pasted image 20221012221622.png]]
* Condition de modulation m
`{Ka*Am}/{Ac} < 1`
* Puissance total d'un signal pt
`pt = pc+pusb+plsb`
* Puissance porteuse pc
`pc = {pt}/{1+({m^2}/{2})}`
* Puissance latéral inférieur et supérieur usb/lsb
```
pusb = plsb = {(m^2)/4}*pc
pusb = plsb = {pt-pc}/{2}
```
* Bande de fréquence (distance entre usb et lsb)
``bf = flsb-fsub``

# Modulation FM
* Variation fréquence  
* Si Am faible alors fs lent
* Si Am elevée alors fs rapide
* Si Am = 0 alors fm = fc
* Quand Am monte s(t) accélère
* Quand Am descend, s(t) décélère
* Indice de modulation m 
```
B = {delta(fm)}/fm
B = {k*Am}/fm
```

# Demodulation
* On obtient m(t) en multipliant s(t).*p(t)