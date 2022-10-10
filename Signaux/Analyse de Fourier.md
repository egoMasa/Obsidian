# Guide Analyse de Fourier

# Objectif : 
* Différence entre Série de Fourier et Transformée de Fourier
* Calculer SF d'un signal
* Calculer TF d'un signal
* Tracer spectre d'un signal (domaine fréquentielle)

# Différence entre Série de Fourier et Transformée de Fourier
* N'importe quel signal peut être une somme de sinusoïdes de fréquence différente. 
* On peut décomposer (séparer) un signal entre plusieurs autres signaux qui une fois additioner donne le signal d'origine
* La TF et SF permet de passer du domaine temporel au domaine fréquentiel car il existe un lien entre les deux
![[Pasted image 20221008163835.png]]
* On appelle harmonique une composante du signal final. Plus il y a d'harmoniques plus le signal est harmonieux (riche en terme de précision)
# Transformées de Fourier
* La TF est utilisé uniquement pour les signaux non périodiques
```
TF(s(t)) = S(f) = s(t)*e^-2*i*pi*f*t
```
* On obtient la représentation fréquentielle (spectre) en calculant le module de la TF du signal temporel
```
Spectre = |S(f)| = |TF(s(t))| 
```
* On peut obtenir le signal temporel s(t) à partir de sa TF (S(f))
```
TF^-1(S(f))= s(t) = S(f)*e^2*i*pi*f*t
```
![[Pasted image 20221008164810.png]]

# Séries de Fourier
* La *SF* est utilisé uniquement pour les ***signaux périodiques***

## Décomposition périodique symétrique
* On peut ***décomposer*** un signal périodique, voici formule générale
```
Ao + (An*cos(n*2*pi*f*t)+Bn*sin(n*2*pi*f*t))

Ao = 1/T * s(t) = Valeur moyenne
An = 2/T * s(t)*cos(n*2pi/T*t) = Intégrale sur 1 periode T
Bn = 2/T * s(t)*sin(n*2pi/T*t) = Intégrale sur 1 periode T
n = |Cn| = 1/2*(sqrt(An**2 + Bn**2)) = rang de l'harmonique
```
* Si s(t) est ***paire***(effet miroir) alors elle se décompose en une ***somme de cosinus*** donc ***Bn = 0***
```
Ao + (An*cos(n*2*pi*f*t) + 0
```
* Si s(t) est ***impaire***(non pliable) alors elle se décompose en une ***somme de sinus*** donc ***An = 0***
```
Ao + 0 +Bn*sin(n*2*pi*f*t))
```
* Si s(t) a une ***double symétrie*** alors tous les ***harmoniques de rang pair*** sont ***nuls***

## Calcul de coefficients
* Ao = Valeur moyenne du signal sur 1 periode (valeur où signal centré)
```
1/T * s(t)
ou A/2
```
* An = Intégrale sur une période T
```
2/T * s(t)*cos(n*2pi/T*t)
```
* Bn = Intégrale sur une période T
```
2/T * s(t)*cos(n*2pi/T*t)
```

## Analyse d'un signal 
* Calcul valeur moyenne Ao
`Ao = A/2`
* Recherche symétrie (pair ou impaire)
`Paire : Bn = 0` `Impair : An = 0`
* Recherche si double symétrie ou pas
`Oui : Harmoniques paires = 0`
* Calcul des coefficients
	* ***BasicMethode*** : sans simplification de l'intervalle d'intégration, on calcule l'intégrale sur un période entière
	* ***OneSymetryMethode*** : On prend que la symétrie principale. On divise par 2 l'intervalle et on multiple par 2 le resultat.
	* ***TotalSymetryMethode*** : On prend la symétrie principale + double symétrie sur signal sans sa valeur moyenne. On divise par 2 l'intervalle et on multiple par 2 le résultat.
* Mise sous forme SF
* Calcul coefficients complexe (pour obtenir spectre)
* Tracé du spectre (domaine fréquentiel)
# I) Series de Fourier (réelles/complexes)
* Equation d'un signal sinusoidale :
`f(t) = Asin(2*pi*f*t+phi)+Offset`
