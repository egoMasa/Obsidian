```matlab
%Transformée de Fourier : Passer du domaine temporel au domaine
%fréquentielle
clear all
close all
clc
%Etape 1 : Définir paramètre de base
frequence_signal = 1;
periode_signal = 1/frequence_signal;
nombre_echantillions = 1e3;
axe_t = linspace(-5*periode_signal,5*periode_signal,nombre_echantillions); %On va avoir 10 périodes
signal_temporel = sin(2*pi*frequence_signal*axe_t);

%Etape 2 : Affichage signal echantilloné
figure(1)
plot(axe_t,signal_temporel,'linewidth',2)
grid on

%Etape 3 : FFT
periode_echantillonage = axe_t(2)-axe_t(1); %axe_t(n)-axe_t(n-1)
frequence_echantillonnage = 1/periode_echantillonage;
resolution_frequentielle = 2^18;
transforme_fourier = fft(signal_temporel,resolution_frequentielle)
axe_f = linspace(-frequence_echantillonnage/2,frequence_echantillonnage/2,resolution_frequentielle)
%axe_f = linspace(-frequence_echantillonnage/2,frequence_echantillonnage/2,length(transforme_fourier))

%Etape 4 : Affichage transformée de Fourier (Dyrac symétrique ici)
figure;
%plot(axe_f,real(fftshift(transforme_fourier)),'linewidth',2);grid on;hold on;
%plot(axe_f,imag(fftshift(transforme_fourier)),'r','linewidth',2);grid on;
plot(axe_f,abs(fftshift(transforme_fourier)),'g','linewidth',2);grid on%Important
xlabel('Fréquence');ylabel('Amplitude');
xlim([-10*frequence_signal 10*frequence_signal]);

%Cas particulier : signal_temporel est une somme de 3 sinus
signal_temporel = sin(2*pi*frequence_signal*axe_t)+0.3*sin(2*pi*3*frequence_signal*axe_t)+0.1*sin(2*pi*5*frequence_signal*axe_t);
periode_echantillonage = axe_t(2)-axe_t(1);
frequence_echantillonnage = 1/periode_echantillonage;
resolution_frequentielle = 2^18;
transforme_fourier = fft(signal_temporel,resolution_frequentielle)
axe_f = linspace(-frequence_echantillonnage/2,frequence_echantillonnage/2,resolution_frequentielle)
figure;
plot(axe_f,abs(fftshift(transforme_fourier)),'g','linewidth',2);grid on%Important
xlabel('Fréquence');ylabel('Amplitude');
xlim([-10*frequence_signal 10*frequence_signal]);
```