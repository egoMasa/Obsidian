```matlab
close all
clear all
clc

%Fréquence d'échantillonage fe
fe = 4000 %Hz
%Periode d'échantillonnage Te
Te = 1/fe

%Periode d'échantillonnage t du signal 
t = 0:Te:1; %De 0 à Te avec pas de 1
%Fréquence du signal 
f0 = 50;
%Signal sinusoidale
s = sin(2*pi*f0*t);
figure(1)
plot(t,s)
figure(2)
stem(s,'r')%Affichage particulier
```