# Cours chaine de transmissions

# I) Shéma transmissions numériques
* Le ***signal transportant*** une information doit passer par un moyen de transmission entre un ***émetteur*** et un ***récepteur***
* Il faut souvent ***adapté le signal*** par rapport au ***canal de transmission*** (Hertzien, Filaire, Optique)
* La modulation est le processus qui ***transforme le signal d'origine*** (à transmettre) en une forme adapté au canal de transmission. 
* La modulation consiste à faire ***varier l'amplitude, la fréquence*** et/ou la ***phase*** d'un ***signal sinusoidale*** nommée ***porteuse***
* L'équipement qui effectue cette modulation s'appelle un modulateur
* La demodulation consiste à extraire le signal d'origine de la porteuse.

	![[Amfm3-en-de.gif]]


# II) Modulation

### 1) Modulation d'amplitude (linéaire) deux bandes latérales avec porteuse(AM)
* On prend 2 signaux d'entrée : 

* 1) Signal à transmettre (basse fréquence) appelé modulante
```
m(t) = Am*cos(2*pi*Fm*t + Pm) = Am*cos(Wm*t+Pm)
```

* 2)Signal porteuse (haute fréquence)  
```
p(t) = Ac*cos(2*pi*Fc*t + Pc) = Ac*cos(Wc*t+Pc)
```

* On sait que ``Pm = Pc``
```
Am = Amplitude onde modulé ( à transmettre)
Fm = Fréquence onde modulé
Wm = 2*pi*Fm
Pm = Phase modulante
```

*  On sort 1 signal en sortie = Signal modulé qui est ``m(t)*p(t)``
```
f(t) = [Ac+Ka*m(t)]*cos(Wc*t)
	Ac= Amplitude de la porteuse (carrier)
	Ka = Coefficient 
	K = Régler au choix, selon le cas
```

* Condition de modulation (Ampli_porteuse superieure au signal * K)
```
Ac >= Ka*mfm(t)
```
* Indice de modulation 
```
m = Ka*Am
```
* Condition modulation 
```
m = (Ka*Am)/Ac <= 1
```
* Pour Analyser un signal il faut linéariser (multiplication vers addition)	
```
cos(a)*cos(b) = (cos(a+b)+cos(a-b))/2
```
* Puissance d'emission signal modulé en W : 
```
pT = pC+pUSB+pLSB
pC(porteuse) = pT/1+(m^^2)/2
pUSB = PLSB = ((m^^2)/4)*pC
```

### 2) Modulation d'amplitude (linéaire) deux bandes latérales sans porteuse


### 3) Modulation d'amplitude (linéaire) bande laterale unique


### 4) Modulation fréquence/phase (non-linéaire)(FM)
* Message : ``m(t)``
* Porteuse : ``p(t)``
* Signal modulé : ``s(t) = a(t)*cos(2*pi*fo*t + phi(t))``
* 




# III) Démodulation

### 1) Démodulation double bande avec porteuse (Dbap)
* Pour retrouver le signal d'origine on s'attarde sur l'enveloppe si elle est positive

### 2) Démodulation double bande sans porteuse (Dbsp)
* On transmet seulement les deux bandes latérales
```
f(t) = Ac*cos(Wm)*t*cos(Wc)*t = Ka*Am*cos(Wm)*t*cos(wC)*t
```
* Pour obtenir le signal démodulé "Sd(t)" il faut appliquer un filtre passe bas du signal modulé multiplié par la porteuse
```
Soit s(t)= s_modulé | p(t) = s_porteuse

Sd(t) = FiltrePB{s(t)*p(t)}
Sd(t) = [m(t)*Acos(2*pi*fo*t)]*Acos(2*pi*fo*t)
Sd(t) = ((A**2)/2)*[m(t)+m(t)*cos(2*pi*(2*fo)*t]
Sd(t) = m(t)
```
* Cas où le signal modulé pas en phase avec porteur 
```
Sd(t) = s(t)*p(t+cos(phi))
Sd(t) = [m(t)*Acos(2*pi*fo*t)]*Acos(2*pi*fo*t+phi)
Sd(t) = ((A**2)/2)*[m(t)*cos(phi)+m(t)*cos(2*pi*(2*fo)*t+phi]
Sd(t) = m(t)*cos(phi)

Attention si phi = pi/2 on perd le signal
```

# Résumé exercice

* Trouver fréquence  fo à partir `Acos((3,802*10**6)*t)`
```
Fo = (3,802*10**6)/2*pi
```
* Trouver fréquence modulante (distance entre porteuse et bande latérale)
```
Fréquence/bande modulante = Fp-Fusb
```
* Trouver indice de modulation %
```
m = Am/(Ap/2)
```
* Trouver indice de modulation à partir P(t)
```
m = sqrt((P(t)/Pc)-1)*2
```
* Trouver bande fréquence d'émission (distance entre USB et LSB)
```
Bande de fréquence = Fusb-Flsb
```
* Trouver Pusb | Plsb | Pp | Pt
```
Pt = Pc(1+(m**2/2))
Pc = Pt/1,(m**2/2)
Pusb = Plsb = ((m**2)/4)*Pc
Pusb = Plsb = (Pt-Pc)/2
```
* Trouver Pssbtc
```
Pssbtc = Pc+Pusb
```

