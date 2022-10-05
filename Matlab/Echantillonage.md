```matlab
clear all
close all
clc
%Signal Analogique 
a = 0:0.125:5;
signal = exp(-a).*sin(a.^2);
subplot(4,1,1)%Pour l'affichage(comment spliter une figure)(nb_vertical,longeur,index_vertical)
plot(a,signal,'r')
legend('Signal analogique');
grid on

%Signal echantilloné
subplot(4,1,2)%Pour l'affichage pas important
stem(a,signal,'r')
legend('Signal echantilloné');
grid on

%Signal quantifié
subplot(4,1,3)%Pour l'affichage pas important
plot(a,signal,'g')
legend('Signal quantifié');
grid on

%Transformée de Fourier domaine tempo
subplot(4,1,4)%Pour l'affichage pas important
fourier = abs(fft(signal))%Transformé de Fourier de z (fft = fastFourierTransfo)
plot(a,fourier,'b')
legend('Transformée de Fourier ');
grid on

%Transformée de Fourier domaine fréquentiel
x = a/signal
Tf = abs(fft(signal));
figure(2)
plot(a,signal,'r')
title('Transformée de Fourier')
```